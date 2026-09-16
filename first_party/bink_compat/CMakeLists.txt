option(ARCANUM_BINK_FFMPEG "Use the FFmpeg backend for Bink video playback (LGPL, dynamically linked)" OFF)

if(ARCANUM_BINK_FFMPEG)
    find_package(PkgConfig REQUIRED)
    pkg_check_modules(FFMPEG REQUIRED
        libavcodec
        libavformat
        libavutil
        libswscale
        libswresample
    )

    add_library(bink_compat STATIC
        "include/bink_compat.h"
        "src/bink_compat_ffmpeg.c"
    )

    # Use FFmpeg headers for types and version macros, but do NOT link the
    # libraries. They are loaded lazily at runtime via dlopen so the game still
    # launches (and simply skips movies) when FFmpeg is not installed.
    #
    # Pass the pkg-config library directory so dlopen can find the libraries
    # even when they live outside the loader's default search path (e.g.
    # Homebrew on macOS). The loader's default path is tried as a fallback.
    pkg_get_variable(FFMPEG_LIBDIR libavcodec libdir)

    # Fork change. pkg-config on Homebrew reports the versioned Cellar directory of the
    # BUILD machine, e.g. /opt/homebrew/Cellar/ffmpeg/9.0.1_1/lib. That path encodes an exact
    # formula revision and does not exist on anybody else's Mac: a `_1` revision bump is
    # enough to miss. Measured, not guessed: the first build baked 9.0.1_1 and the machine it
    # was for had plain 9.0.1, so every dlopen fell through to the bare SONAME.
    #
    # The bare-name fallback does not rescue it on Apple Silicon either. macOS only searches
    # $HOME/lib:/usr/local/lib:/lib:/usr/lib by default, and Homebrew on arm64 is under
    # /opt/homebrew. (Intel Homebrew IS /usr/local, which is why this only bites on arm64.)
    #
    # ARCANUM_FFMPEG_LIBDIR lets the build name the stable prefix symlink directory instead,
    # which survives formula revisions.
    if(ARCANUM_FFMPEG_LIBDIR)
        set(FFMPEG_LIBDIR "${ARCANUM_FFMPEG_LIBDIR}")
    endif()
    target_include_directories(bink_compat PRIVATE ${FFMPEG_INCLUDE_DIRS})
    target_compile_definitions(bink_compat PRIVATE "FFMPEG_LIBDIR=\"${FFMPEG_LIBDIR}\"")
    target_link_libraries(bink_compat PRIVATE SDL3::SDL3 ${CMAKE_DL_LIBS})
else()
    add_library(bink_compat STATIC
        "include/bink_compat.h"
        "src/bink_compat.c"
    )
endif()

target_include_directories(bink_compat PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")

set(BINK_COMPAT_LIBRARY "bink_compat" PARENT_SCOPE)
set(BINK_COMPAT_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/include" PARENT_SCOPE)
