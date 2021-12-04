---
title: "Compiling an SDL2 Game to WASM"
date: 2020-07-13T02:59:12-04:00
---
![](/images/snake.png)  

I have a Snake clone written in C++ and SDL2, and while it's not just-add-water (yet) to compile a game to WASM with emscripten, it's pretty close. There was just some minor refactoring to be done, but it didn't affect the desktop version.


# Infinite loop error
My game loop spins forever or until a quit event occurs. Infinite loops don't fly in the browser however, and this is the biggest change that had to be made to my game.

I had to move some code around, and what once sat in a while loop now lives in a function.

Before (no good):
```
// main.cpp

int main()
{
	<init code...>

	while(running)
	{
		<game loop code...>
	}

	return 0;
}
```

After (good):
```
// main.cpp

<declarations, defined later during init...>

void mainloop()
{
	<game loop code...>

	if(quit)
	{
		emscripten_cancel_main_loop(); 
		// ^ Add this to your close and destroy code 
		<close sdl subsystems and destroy...>
	}
}

int main()
{
	<init code...>

	emscripten_set_main_loop(mainloop, 0, 1);

	return 0;
}
```
My main routine now ends with the `emscripten_set_main_loop` call, whereas prior to this WASM refactor, it ended with a closing of SDL2 subsystems, meaning my game now handles quitting at the end of the game loop.

Later on I'll add define guards to make sure this all still compiles for desktop.

# Build Flags

There were some build troubles. To be fair, I didn't read all that much on what these build flags mean, but that's alright because it builds now and that's all that matters (just kidding). It does build though.


### `RuntimeError error: unreachable`

A Google search led to a suggestion that this resulted from control flow not being able to reach code due to a missing return statement? I then tracked it down to a SDL_RenderPresent call. I'm wrapping most SDL things in my own namespace and functions so I though that might be the problem. It turns out it was the `asyncify` flag which I thoughtlessly and needlessly added to the build flags.

Bad:
```
em++ *.cpp -o snake.html -g -lm --bind -s USE_SDL=2 -s USE_SDL_IMAGE=2 -s SDL2_IMAGE_FORMATS='["png"]' --preload-file assets/ --use-preload-plugins -s ASYNCIFY
```

Good:
```
em++ *.cpp -o snake.html -g -lm --bind -s USE_SDL=2 -s USE_SDL_IMAGE=2 -s SDL2_IMAGE_FORMATS='["png"]' --preload-file assets/ --use-preload-plugins
```
No asyncify.

### Handling Images
`-s SDL2_IMAGE_FORMATS='["png"]' --preload-file assets/ --use-preload-plugins`

These flags will let SDL2_image load PNG's. Also, a trailing slash must be used to preload an entire directory with `preload-file`.


# Maintaining Interoperability
Makefile targets look like this, snake-game being for desktop and snake.html for wasm:
```
snake-game: *.cpp
        clang++ -std=c++11 -I /usr/include/SDL2/ -l SDL2 -l SDL2_image $^ -o $@

snake.html: *.cpp
        em++ $^ -o $@ -g -lm --bind -s USE_SDL=2 -s USE_SDL_IMAGE=2 -s SDL2_IMAGE_FORMATS='["png"]' --preload-file assets/ --use-preload-plugins 

```

I also added define guards around emscripten function calls and the header as well.
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

	<close SDL subsystems and destroy...>
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
	// repeatedly calling mainloop on desktop
        while(!quit) mainloop();
        #endif

        return 0;
}

```

# Game Development with WASM in Mind

I will probably separate my code like the above in the future to keep it open for WASM compilation, much like we now develop webpages mobile-first.

If you've made it this far, you'd may as well play my Snake clone here: http://mattconndev.com/snake-wasm/

# Sources

Code restructuring for emscripten: https://medium.com/@robaboukhalil/porting-games-to-the-web-with-webassembly-70d598e1a3ec

Define guards for interoperability: https://dev.to/kibebr/i-made-a-game-in-c-run-in-a-web-browser-and-so-can-you-4deb
