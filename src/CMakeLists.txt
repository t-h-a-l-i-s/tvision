cmake_minimum_required(VERSION 2.8 FATAL_ERROR)
project(tvision VERSION 0.8 LANGUAGES CXX)

include(GNUInstallDirs)

add_subdirectory(lib)
#add_subdirectory(demo)
#add_subdirectory(tutorial)
#add_subdirectory(doc)
#add_subdirectory(tvhc)

set(EXTRA_DIST Announce COPYRIGHT color-xterm.ti) 
