cmake_minimum_required(VERSION 3.0)
project(cmark-gfm)

set(PROJECT_VERSION_MAJOR 0)
set(PROJECT_VERSION_MINOR 29)
set(PROJECT_VERSION_PATCH 0)
set(PROJECT_VERSION_GFM 2)
set(PROJECT_VERSION ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}.${PROJECT_VERSION_PATCH}.gfm.${PROJECT_VERSION_GFM})

include("FindAsan.cmake")
include("CheckFileOffsetBits.cmake")

if("${CMAKE_SOURCE_DIR}" STREQUAL "${CMAKE_BINARY_DIR}")
    message(FATAL_ERROR "Do not build in-source.\nPlease remove CMakeCache.txt and the CMakeFiles/ directory.\nThen: mkdir build ; cd build ; cmake .. ; make")
endif()

option(CMARK_TESTS "Build cmark-gfm tests and enable testing" ON)
option(CMARK_STATIC "Build static libcmark-gfm library" ON)
option(CMARK_SHARED "Build shared libcmark-gfm library" ON)
option(CMARK_LIB_FUZZER "Build libFuzzer fuzzing harness" OFF)

add_subdirectory(src)
add_subdirectory(extensions)
if(CMARK_TESTS AND (CMARK_SHARED OR CMARK_STATIC))
  add_subdirectory(api_test)
endif()
add_subdirectory(man)
if(CMARK_TESTS)
  enable_testing()
  add_subdirectory(test testdir)
endif()

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE "Release" CACHE STRING
  "Choose the type of build, options are: Debug Profile Release Asan Ubsan." FORCE)
endif(NOT CMAKE_BUILD_TYPE)
