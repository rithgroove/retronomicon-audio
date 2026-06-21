cmake_minimum_required(VERSION 3.15)
project(retronomicon-audio VERSION 0.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

if(NOT TARGET retronomicon)
    message(FATAL_ERROR "retronomicon-audio must be built from the workspace after retronomicon")
endif()

set(RETRONOMICON_WORKSPACE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/..)

if(RETRONOMICON_AUDIO_BACKEND STREQUAL "SDL_MIXER" AND NOT (TARGET SDL2 AND TARGET SDL2_mixer))
    message(FATAL_ERROR "RETRONOMICON_AUDIO_BACKEND=SDL_MIXER requires RETRONOMICON_GRAPHICS_BACKEND=SDL until SDL_mixer is vendored directly in retronomicon-audio")
endif()

if(TARGET SDL2 AND TARGET SDL2_mixer)
    add_library(retronomicon-audio-sdl-mixer
        ${RETRONOMICON_WORKSPACE_DIR}/retronomicon-sdl/src/audio/sdl_audio_player.cpp
        ${RETRONOMICON_WORKSPACE_DIR}/retronomicon-sdl/src/asset/sdl_music_asset.cpp
        ${RETRONOMICON_WORKSPACE_DIR}/retronomicon-sdl/src/asset/sdl_sound_effect_asset.cpp
    )

    target_include_directories(retronomicon-audio-sdl-mixer
        PUBLIC
            ${RETRONOMICON_WORKSPACE_DIR}/retronomicon/include
            ${RETRONOMICON_WORKSPACE_DIR}/retronomicon-sdl/include
            ${RETRONOMICON_WORKSPACE_DIR}/retronomicon-sdl/external/SDL2/include
            ${RETRONOMICON_WORKSPACE_DIR}/retronomicon-sdl/external/SDL2_mixer/include
    )

    target_link_libraries(retronomicon-audio-sdl-mixer
        PUBLIC
            retronomicon
            SDL2
            SDL2_mixer
    )

    target_compile_definitions(retronomicon-audio-sdl-mixer
        PUBLIC RETRONOMICON_AUDIO_BACKEND_SDL_MIXER=1
    )
else()
    add_library(retronomicon-audio-sdl-mixer INTERFACE)
    target_link_libraries(retronomicon-audio-sdl-mixer INTERFACE retronomicon)
    target_compile_definitions(retronomicon-audio-sdl-mixer INTERFACE RETRONOMICON_AUDIO_BACKEND_SDL_MIXER=1)
endif()

if(RETRONOMICON_AUDIO_BACKEND STREQUAL "OPENAL" AND NOT TARGET OpenAL)
    message(FATAL_ERROR "RETRONOMICON_AUDIO_BACKEND=OPENAL requires RETRONOMICON_GRAPHICS_BACKEND=OPENGL until OpenAL is vendored directly in retronomicon-audio")
endif()

if(TARGET OpenAL)
    add_library(retronomicon-audio-openal
        ${RETRONOMICON_WORKSPACE_DIR}/retronomicon-opengl/src/audio/openal_audio_player.cpp
        ${RETRONOMICON_WORKSPACE_DIR}/retronomicon-opengl/src/asset/openal_music_asset.cpp
        ${RETRONOMICON_WORKSPACE_DIR}/retronomicon-opengl/src/asset/openal_sound_effect_asset.cpp
    )

    target_include_directories(retronomicon-audio-openal
        PUBLIC
            ${RETRONOMICON_WORKSPACE_DIR}/retronomicon/include
            ${RETRONOMICON_WORKSPACE_DIR}/retronomicon/external/stb
            ${RETRONOMICON_WORKSPACE_DIR}/retronomicon-opengl/include
            ${RETRONOMICON_WORKSPACE_DIR}/retronomicon-opengl/external/openal/include
    )

    target_link_libraries(retronomicon-audio-openal
        PUBLIC
            retronomicon
            OpenAL
    )

    target_compile_definitions(retronomicon-audio-openal
        PUBLIC RETRONOMICON_AUDIO_BACKEND_OPENAL=1
    )
else()
    add_library(retronomicon-audio-openal INTERFACE)
    target_link_libraries(retronomicon-audio-openal INTERFACE retronomicon)
    target_compile_definitions(retronomicon-audio-openal INTERFACE RETRONOMICON_AUDIO_BACKEND_OPENAL=1)
endif()

add_library(retronomicon-audio-xaudio2 INTERFACE)
target_link_libraries(retronomicon-audio-xaudio2 INTERFACE retronomicon)
target_compile_definitions(retronomicon-audio-xaudio2 INTERFACE RETRONOMICON_AUDIO_BACKEND_XAUDIO2=1)

message(STATUS "Building retronomicon-audio backend targets")
