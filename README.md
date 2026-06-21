# retronomicon-audio

Audio backend boundary for Retronomicon.

This module owns audio backend targets independently from graphics backends:

- `retronomicon-audio-sdl-mixer`
- `retronomicon-audio-openal`
- `retronomicon-audio-xaudio2`

The current targets are CMake scaffolds for workspace selection. Move concrete
SDL_mixer, OpenAL, and XAudio2 `IAudioPlayer` implementations here as the next
step so graphics modules do not imply an audio backend.
