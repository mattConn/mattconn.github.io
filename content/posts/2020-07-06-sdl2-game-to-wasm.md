---
title: "2020 07 06 SDL2 Game to Wasm"
date: 2020-07-06T02:59:12-04:00
draft: true
---

# Infinite loop error

Need to move code:
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