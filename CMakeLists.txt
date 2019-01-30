cmake_minimum_required (VERSION 2.8)
project (SDL_gfx)
set (SDL_gfx_VERSION_MAJOR 3)
set (SDL_gfx_VERSION_MINOR 0)


set(ENV{PKG_CONFIG_PATH} "${CMAKE_INSTALL_PREFIX}/lib/pkgconfig/")
include(FindPkgConfig)
pkg_check_modules(PKG_SDL2 REQUIRED sdl2)

if(NOT (WINDOWS OR CYGWIN))
  set(prefix ${CMAKE_INSTALL_PREFIX})
  set(exec_prefix "\${prefix}")
  set(libdir "\${exec_prefix}/lib${LIB_SUFFIX}")
  set(bindir "\${exec_prefix}/bin")
  set(includedir "\${prefix}/include")
endif()

configure_file("${SDL_gfx_SOURCE_DIR}/SDL_gfx.pc.in" "${SDL_gfx_BINARY_DIR}/SDL_gfx.pc" @ONLY)


include_directories (${PKG_SDL2_INCLUDE_DIRS})
link_directories(${PKG_SDL2_LIBRARY_DIRS})

file(GLOB SOURCE_FILES ${CMAKE_SOURCE_DIR}/*.c)
file(GLOB INCLUDE_FILES ${CMAKE_SOURCE_DIR}/*.h)

add_library (SDL_gfx STATIC	${SOURCE_FILES}	${INCLUDE_FILES})
target_link_libraries (SDL_gfx ${PKG_SDL2IMAGE_LIBRARIES} )

install (TARGETS SDL_gfx DESTINATION lib)
install (FILES ${INCLUDE_FILES} DESTINATION include/SDL2)

if(NOT (WINDOWS OR CYGWIN))
  if(FREEBSD)
    # FreeBSD uses ${PREFIX}/libdata/pkgconfig
    install(FILES ${SDL_gfx_BINARY_DIR}/SDL_gfx.pc DESTINATION "libdata/pkgconfig")
  else()
    install(FILES ${SDL_gfx_BINARY_DIR}/SDL_gfx.pc
      DESTINATION "lib${LIB_SUFFIX}/pkgconfig")
  endif()
endif()
