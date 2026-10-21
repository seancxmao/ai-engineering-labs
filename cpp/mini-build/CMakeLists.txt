cmake_minimum_required(VERSION 3.20)

project(NativeBuildLab LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
SET(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(math_utils_lib src/math_utils.cpp)

target_include_directories(math_utils_lib
    PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/include
)

add_executable(hello_native src/main.cpp)

target_link_libraries(hello_native
    PRIVATE
        math_utils_lib
)

