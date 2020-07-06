---
title: "2020 07 06 SDL2 Game to Wasm"
date: 2020-07-06T02:59:12-04:00
draft: true
---

# Infinite loop error

Need to move code:
https://medium.com/@robaboukhalil/porting-games-to-the-web-with-webassembly-70d598e1a3ec
Before (no good):
```
int main()
{
	init code...

	while(running)
	{
		game loop code...
	}

	return 0;
}
```

After (good):
```
declarations...

void mainloop()
{
	game loop code...

	if(quit)
	{
		emscripten_cancel_main_loop();
		close sdl subsystems and destroy...
	}
}

int main()
{
	init code...

	emscripten_set_main_loop(mainloop, 0, 1);

	return 0;
}
```

# Build Flags

RuntimeError error: unreachable

From control flow not being able to reach code due to missed returned statement? I tracked it down to a SDL_RenderPresent call. I'm wrapping most SDL things in my own namespace and functions so I though that might be the problem. It turns out it was the asyncify flag.

Bad:
```
em++ *.cpp -o snake.html -g -lm --bind -s USE_SDL=2 -s USE_SDL_IMAGE=2 -s SDL2_IMAGE_FORMATS='["png"]' --preload-file assets/ --use-preload-plugins -s ASYNCIFY
```

Good:
```
em++ *.cpp -o snake.html -g -lm --bind -s USE_SDL=2 -s USE_SDL_IMAGE=2 -s SDL2_IMAGE_FORMATS='["png"]' --preload-file assets/ --use-preload-plugins
```
No asyncify.

# Maintaining interoperability
Makefile targets look like this, snake-game being for desktop and snake.html for wasm:
```
snake-game: *.cpp
        clang++ -std=c++11 -I /usr/include/SDL2/ -l SDL2 -l SDL2_image $^ -o $@

snake.html: *.cpp
        em++ $^ -o $@ -g -lm --bind -s USE_SDL=2 -s USE_SDL_IMAGE=2 -s SDL2_IMAGE_FORMATS='["png"]' --preload-file assets/ --use-preload-plugins 

```

I also added define guards around emscripten function calls and the header as well.
https://dev.to/kibebr/i-made-a-game-in-c-run-in-a-web-browser-and-so-can-you-4deb
At the top of main.cpp:

```
.
.
.

#ifdef __EMSCRIPTEN__
#include "emscripten.h"
#endif

.
.
.
```
and, towards the end of mainloop():

```
.
.
.

if(quit) {
	#ifdef __EMSCRIPTEN__
	emscripten_cancel_main_loop();
	#endif

	SDLw::close(); // My wrapper function/namespace
}

.
.
.
```
and at the bottom of main():
```
.
.
.

        #ifdef __EMSCRIPTEN__
        emscripten_set_main_loop(mainloop, 0, 1);
        #endif

        #ifndef __EMSCRIPTEN__
        while(!quit) mainloop();
        #endif

        return 0;
}

```

