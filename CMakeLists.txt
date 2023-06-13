cmake_minimum_required(VERSION 3.21)
project(FMCA CXX)

message(STATUS "
****************************************************************************
*                  ______   __    __    ______    ______                   * 
*                 |   ___| |  \\  /  |  /   ___|  /      \\                  *
*                 |   __   |   \\/   | |   /     |   /\\   |                 *
*                 |   __|  |        | |  |      |  |__|  |                 *
*                 |  |     |  |\\/|  | |  |      |   __   |                 *
*                 |  |     |  |  |  | |   \\___  |  |  |  |                 *
*                 |__|     |__|  |__|  \\______| |__|  |__|                 *
*                                                                          *
****************************************************************************
This is FMCA, the Fast Multiresolution Covariance Analysis package.

Copyright (c) 2022, Michael Multerer
All rights reserved.

This source code is subject to the GNU Affero General Public License v3.0
license and without any warranty, see <https://github.com/muchip/FMCA>
for further information.
")

option(FMCA_WITH_SANITIZER "build FMCA using address sanitzer flags" OFF)

function(add_sanitizer_flags target)
    if(FMCA_WITH_SANITIZER)
      target_compile_options(
        ${target}
        PRIVATE -fsanitize=address
                -fsanitize=undefined
                -fno-sanitize-recover=all
                -fsanitize=float-divide-by-zero
                -fsanitize=float-cast-overflow
                -fno-sanitize=null
                -fno-sanitize=alignment
      )
      target_link_options(
        ${target}
        PRIVATE
        -fsanitize=address
        -fsanitize=undefined
        -fno-sanitize-recover=all
        -fsanitize=float-divide-by-zero
        -fsanitize=float-cast-overflow
        -fno-sanitize=null
        -fno-sanitize=alignment
      )
    endif()
endfunction()

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")
find_package(Eigen3 REQUIRED)
find_package(Python COMPONENTS Interpreter Development)
find_package(pybind11 CONFIG)

include_directories(${EIGEN3_INCLUDE_DIR})
message("${CMAKE_MODULE_PATH}")
message("${EIGEN3_INCLUDE_DIR}")

set (FMCA_PATH ${PROJECT_SOURCE_DIR})
message("${PROJECT_SOURCE_DIR}")

set(CMAKE_CXX_FLAGS "-std=c++11 -I${FMCA_PATH}")
set(CMAKE_CXX_FLAGS_DEBUG "-g")
set(CMAKE_CXX_FLAGS_RELEASE "-O3")

set( PROJECT_SOURCE_DIR "${PROJECT_SOURCE_DIR}")
include_directories("${PROJECT_SOURCE_DIR}")

add_subdirectory(tests)
add_subdirectory(py)
enable_testing()
