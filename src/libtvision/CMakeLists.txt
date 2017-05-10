cmake_minimum_required(VERSION 3.0)
project(tvision VERSION 0.8)

include(CheckIncludeFiles)

# Configuration
option(enable-russian "enable Russian character set (now obsolete)" off)
option(disable-acs "disable alternate character set" off)
option(disable-8bit "disable 8 bit output" off)
option(disable-linux-gpm "disable Linux GPM mouse support" on)
option(disable-linux-vcs "disable Linux VCS support" on)
option(disable-freebsd-mouse "disable FreeBSD mouse support" on)

if(enable-russian)
    message(STATUS "Russian character set ... enabled")
    set(ENABLE_RUSSIAN_CHARSET 1)
endif(enable-russian)

if(disable-acs)
    message(STATUS "Alternative character set ... disabled")
    set(DISABLE_ACS 1)
endif(disable-acs)

if(disable-8bit)
    message(STATUS "8 bit output ... disabled")
    set(DISABLE_8BIT_CHARS 1)
endif(disable-8bit)

if(NOT disable-linux-gpm)
    message(STATUS "Linux GPM mouse support ... enabled")
    set(ENABLE_GPM 1)
    find_library(HAVE_LIBGPM gpm)
    check_include_files("gpm.h" HAVE_GPM_H)
endif(NOT disable-linux-gpm)

if(NOT disable-linux-vcs)
    message(STATUS "Linux VCS support ... enabled")
    set(ENABLE_VCS 1)
endif(NOT disable-linux-vcs)

if(NOT disable-freebsd-mouse)
    message(STATUS "FreeBSD mouse support ... enabled")
    set(ENABLE_FBSDM 1)
endif(NOT disable-freebsd-mouse)

# visibility
SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fvisibility=hidden")

# Platform Checks
check_include_files("dlfcn.h;inttypes.h;stdint.h;stdlib.h;strings.h;string.h" hasStdLibs)
if(hasStdLibs)
    message(STATUS "ANSI C header files ... found")
    set(STDC_HEADERS 1)
    set(HAVE_DLFCN_H 1)
    set(HAVE_INTTYPES 1)
    set(HAVE_STDINT_H 1)
    set(HAVE_STDLIB_H 1)
    set(HAVE_STRINGS_H 1)
    set(HAVE_STRING_H 1)
else(hasStdLibs)
    message(STATUS "ANSI C header files ... not found")
    set(STDC_HEADERS 0)
endif(hasStdLibs)

find_package(Curses)
set(HAVE_CURSES_H CURSES_HAVE_CURSES_H)
set(HAVE_NCURSES_H CURSES_HAVE_NCURSES_H)
if(CURSES_FOUND)
    include_directories("${CURSES_INCLUDE_DIR}")
    find_library(HAVE_LIBCURSES curses)
    find_library(HAVE_LIBNCURSES ncurses)
endif(CURSES_FOUND)

check_include_files("memory.h" HAVE_MEMORY_H)
check_include_files("sys/stat.h" HAVE_SYS_STAT_H)
check_include_files("sys/types.h" HAVE_SYS_TYPES_H)
check_include_files("unistd.h" HAVE_UNISTD_H)

set(CMAKE_BINARY_DIR ${CMAKE_SOURCE_DIR}/build)

set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR})
set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR})

set (PROJECT_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/include)
set (PROJECT_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)

aux_source_directory(${PROJECT_SOURCE_DIR} LIB_SRCS)

include_directories("${PROJECT_BINARY_DIR}")
include_directories("${PROJECT_INCLUDE_DIR}")

add_library(${PROJECT_NAME} SHARED ${LIB_SRCS})
