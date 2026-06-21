cmake_minimum_required(VERSION 3.15)
project(retronomicon-audio VERSION 0.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if(NOT TARGET retronomicon)
    message(FATAL_ERROR "retronomicon-audio must be built from the workspace after retronomicon")
endif()

add_library(retronomicon-audio-sdl-mixer INTERFACE)
target_link_libraries(retronomicon-audio-sdl-mixer INTERFACE retronomicon)
target_compile_definitions(retronomicon-audio-sdl-mixer INTERFACE RETRONOMICON_AUDIO_BACKEND_SDL_MIXER=1)

add_library(retronomicon-audio-openal INTERFACE)
target_link_libraries(retronomicon-audio-openal INTERFACE retronomicon)
target_compile_definitions(retronomicon-audio-openal INTERFACE RETRONOMICON_AUDIO_BACKEND_OPENAL=1)

add_library(retronomicon-audio-xaudio2 INTERFACE)
target_link_libraries(retronomicon-audio-xaudio2 INTERFACE retronomicon)
target_compile_definitions(retronomicon-audio-xaudio2 INTERFACE RETRONOMICON_AUDIO_BACKEND_XAUDIO2=1)

message(STATUS "Building retronomicon-audio backend target scaffold")
