# retronomicon-audio

Audio backend boundary for Retronomicon.

This module owns audio backend targets independently from graphics backends:

- `retronomicon-audio-sdl-mixer`
- `retronomicon-audio-openal`
- `retronomicon-audio-xaudio2`

The current targets are CMake scaffolds for workspace selection. Move concrete
SDL_mixer, OpenAL, and XAudio2 `IAudioPlayer` implementations here as the next
step so graphics modules do not imply an audio backend.

## Current Implementation Locations

Concrete audio code has not been moved here yet:

- SDL_mixer player: `retronomicon-sdl/include/retronomicon/audio/sdl_audio_player.h`
  and `retronomicon-sdl/src/audio/sdl_audio_player.cpp`.
- OpenAL player: `retronomicon-opengl/include/retronomicon/audio/openal_audio_player.h`
  and `retronomicon-opengl/src/audio/openal_audio_player.cpp`.
- XAudio2: target scaffold only; no implementation yet.

## Migration Checklist

1. Move each concrete `IAudioPlayer` implementation into this repository under
   backend-specific folders, for example `src/sdl_mixer/` and `src/openal/`.
2. Keep public includes under `include/retronomicon/audio/...`.
3. Link backend dependencies here, not from graphics backend targets.
4. Leave graphics backends responsible only for windows, rendering, textures,
   and raw input.
5. Add a fake audio player test in `retronomicon` core before changing the
   `IAudioPlayer` contract.
