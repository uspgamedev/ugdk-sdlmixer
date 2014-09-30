cmake_minimum_required(VERSION 2.8.11)
project(SDL2-mixer C)

set(SDL2-mixer-sources SDL_mixer/dynamic_flac.c SDL_mixer/dynamic_fluidsynth.c SDL_mixer/dynamic_mod.c
                       SDL_mixer/dynamic_modplug.c SDL_mixer/dynamic_mp3.c SDL_mixer/dynamic_ogg.c
                       SDL_mixer/effect_position.c SDL_mixer/effect_stereoreverse.c SDL_mixer/effects_internal.c
                       SDL_mixer/fluidsynth.c
                       SDL_mixer/load_aiff.c SDL_mixer/load_flac.c SDL_mixer/load_ogg.c SDL_mixer/load_voc.c
                       SDL_mixer/mixer.c
                       SDL_mixer/music.c SDL_mixer/music_cmd.c SDL_mixer/music_flac.c SDL_mixer/music_mad.c 
                       SDL_mixer/music_mod.c SDL_mixer/music_modplug.c SDL_mixer/music_ogg.c
                       SDL_mixer/wavestream.c
                       
                       SDL_mixer/dynamic_flac.h SDL_mixer/dynamic_fluidsynth.h SDL_mixer/dynamic_mod.h 
                       SDL_mixer/dynamic_modplug.h SDL_mixer/dynamic_mp3.h SDL_mixer/dynamic_ogg.h
                       SDL_mixer/effects_internal.h SDL_mixer/fluidsynth.h
                       SDL_mixer/load_aiff.h SDL_mixer/load_flac.h SDL_mixer/load_ogg.h SDL_mixer/load_voc.h
                       SDL_mixer/music_cmd.h SDL_mixer/music_flac.h SDL_mixer/music_mad.h SDL_mixer/music_mod.h SDL_mixer/music_modplug.h SDL_mixer/music_ogg.h
                       SDL_mixer/wavestream.h

                       SDL_mixer/SDL_mixer.h
)

if(MSVC)
    list(APPEND SDL2-mixer-sources SDL_mixer/version.rc)
endif(MSVC)

add_library(SDL2-mixer ${SDL2-mixer-sources})
target_compile_definitions(SDL2-mixer PRIVATE WAV_MUSIC)
target_link_libraries(     SDL2-mixer LINK_PUBLIC SDL2)
target_include_directories(SDL2-mixer PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/SDL_mixer") # TODO: add 'SYSTEM' flag when we have cmake 2.8.12

find_package(Vorbis)
find_package(VorbisFile)
if(VORBIS_FOUND AND VORBISFILE_FOUND)
    target_compile_definitions(SDL2-mixer PRIVATE     OGG_MUSIC)
    target_link_libraries(     SDL2-mixer LINK_PUBLIC ${VORBIS_LIBRARIES} ${VORBISFILE_LIBRARIES})
    target_include_directories(SDL2-mixer PRIVATE     ${VORBIS_INCLUDE_DIRS} ${VORBISFILE_INCLUDE_DIRS})
endif()

