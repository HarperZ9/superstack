# SPDX-License-Identifier: MIT
# Builds the C++ vector runner. Usage:
#   cmake -S tests -B build && cmake --build build --config Release
#   build/run_vectors <repo root>     (build/Release/run_vectors.exe with MSVC)
cmake_minimum_required(VERSION 3.20)
project(superstack_tests CXX)
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(SUPERSTACK_INCLUDE "${CMAKE_CURRENT_SOURCE_DIR}/.." CACHE PATH "Directory holding superstack.hpp")
add_executable(run_vectors run_vectors.cpp)
target_include_directories(run_vectors PRIVATE "${SUPERSTACK_INCLUDE}")
if(MSVC)
  target_compile_options(run_vectors PRIVATE /W4 /WX /permissive- /utf-8 /EHsc)
else()
  target_compile_options(run_vectors PRIVATE -Wall -Wextra -Wpedantic -Werror)
endif()
