cmake_minimum_required(VERSION 3.24)

project(FOX-1.6)

file(GLOB FOX_SOURCE_FILES src/*.cpp src/*.h)

include(CheckIncludeFileCXX)
check_include_file_cxx(dirent.h HAVE_DIRENT_H)
check_include_file_cxx(unistd.h HAVE_UNISTD_H)

include(CheckCXXSymbolExists)
check_cxx_symbol_exists(vsscanf "cstdio" HAVE_VSSCANF)
check_cxx_symbol_exists(strtoll "cstdlib" HAVE_STRTOLL)
check_cxx_symbol_exists(strtoull "cstdlib" HAVE_STRTOULL)

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/config.h.in ${CMAKE_CURRENT_SOURCE_DIR}/include/FXBuildConfig.h)

add_library(FOX-1.6 STATIC ${FOX_SOURCE_FILES})

include(FindOpenGL)
if(OPENGL_FOUND)
    message("GL/GL.h was found - defining 'HAVE_GLU_H'!")
    target_compile_definitions(FOX-1.6 PRIVATE HAVE_GL_H)
endif()
if(OPENGL_GLU_FOUND)
    message("GL/GLU.h was found - defining 'HAVE_GLU_H'!")
    target_compile_definitions(FOX-1.6 PRIVATE HAVE_GLU_H)
endif()

target_include_directories(FOX-1.6 PRIVATE src/)
target_include_directories(FOX-1.6 PUBLIC include/)
target_link_directories(FOX-1.6 PUBLIC lib/)
target_compile_definitions(FOX-1.6 PRIVATE UNICODE)
if(UNIX)
    target_link_libraries(FOX-1.6 X11 Xi Xxf86vm pthread)
endif(UNIX)

set_target_properties(FOX-1.6 PROPERTIES
        LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib)
