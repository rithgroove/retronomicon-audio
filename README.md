# retronomicon-audio

Audio backend boundary for Retronomicon.

This module owns audio backend targets independently from graphics backends:

- `retronomicon-audio-sdl-mixer`
- `retronomicon-audio-openal`
- `retronomicon-audio-xaudio2`

`SDL_MIXER` and `OPENAL` now build as explicit audio targets selected from the
workspace root. `XAUDIO2` remains a compile-definition scaffold until a concrete
Windows implementation exists.

## Current Implementation Locations

Concrete source files still physically live in the graphics backend submodules,
but are no longer linked by the graphics aggregate targets:

- SDL_mixer player: `retronomicon-sdl/include/retronomicon/audio/sdl_audio_player.h`
  and `retronomicon-sdl/src/audio/sdl_audio_player.cpp`.
- OpenAL player: `retronomicon-opengl/include/retronomicon/audio/openal_audio_player.h`
  and `retronomicon-opengl/src/audio/openal_audio_player.cpp`.
- SDL_mixer target: `retronomicon-audio-sdl-mixer`.
- OpenAL target: `retronomicon-audio-openal`.
- XAudio2: `retronomicon-audio-xaudio2` target scaffold only; no implementation yet.

Because third-party dependencies are still vendored by graphics submodules:

- `RETRONOMICON_AUDIO_BACKEND=SDL_MIXER` requires
  `RETRONOMICON_GRAPHICS_BACKEND=SDL`.
- `RETRONOMICON_AUDIO_BACKEND=OPENAL` requires
  `RETRONOMICON_GRAPHICS_BACKEND=OPENGL`.

## Build Examples

```sh
cmake -S . -B build-sdl-audio \
  -DRETRONOMICON_GRAPHICS_BACKEND=SDL \
  -DRETRONOMICON_AUDIO_BACKEND=SDL_MIXER
cmake --build build-sdl-audio --target retronomicon-audio-sdl-mixer

cmake -S . -B build-openal \
  -DRETRONOMICON_GRAPHICS_BACKEND=OPENGL \
  -DRETRONOMICON_AUDIO_BACKEND=OPENAL
cmake --build build-openal --target retronomicon-audio-openal
```

## Migration Checklist

1. Move each concrete `IAudioPlayer` implementation physically into this repository under
   backend-specific folders, for example `src/sdl_mixer/` and `src/openal/`.
2. Vendor or discover SDL2_mixer/OpenAL from this module so audio can be paired
   with any graphics backend.
3. Keep public includes under `include/retronomicon/audio/...`.
4. Leave graphics backends responsible only for windows, rendering, textures,
   and raw input.
5. Add a fake audio player test in `retronomicon` core before changing the
   `IAudioPlayer` contract.

## New Backend Rule

New audio backends should expose one target named
`retronomicon-audio-<backend>` and implement `retronomicon::audio::IAudioPlayer`.
Keep backend-specific dependency headers out of `retronomicon/include` so the
core remains portable.
