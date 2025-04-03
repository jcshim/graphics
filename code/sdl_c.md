### Nuget에서 sdl2를 설치해야 함.
```
#include <SDL.h>

int main(int argc, char* argv[]) {
    SDL_Init(SDL_INIT_VIDEO);
    SDL_Window* w = SDL_CreateWindow("", 100, 100, 400, 400, 0);
    SDL_Renderer* r = SDL_CreateRenderer(w, -1, 0);

    SDL_Event e;
    while (!SDL_PollEvent(&e) || e.type != SDL_QUIT) {
        SDL_SetRenderDrawColor(r, 0, 0, 0, 255); 
        SDL_RenderClear(r);
        SDL_SetRenderDrawColor(r, 0, 255, 0, 255);
        SDL_Rect rect = { 100, 100, 200, 200 };
        SDL_RenderFillRect(r, &rect);
        SDL_RenderPresent(r);
        SDL_Delay(16);
    }
    SDL_DestroyRenderer(r); 
    SDL_DestroyWindow(w); 
    SDL_Quit();
    return 0;
}
```
