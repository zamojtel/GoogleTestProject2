cmake_minimum_required(VERSION 3.14)
project(GoogleTestProject2 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# MyMathLib static library
add_library(MyMathLib MyMathLib/MyMathLib.cpp)
target_include_directories(MyMathLib PUBLIC MyMathLib)

# Main executable
add_executable(GoogleTestProject2 GoogleTestProject2/GoogleTestProject2.cpp)
target_link_libraries(GoogleTestProject2 PRIVATE MyMathLib)

# GoogleTest: Use system package or FetchContent (recommended for portability)
include(FetchContent)
FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/refs/tags/v1.14.0.zip
)
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

enable_testing()

# MathTestProject (unit tests)
add_executable(MathTestProject MathTestProject/test.cpp)
target_link_libraries(MathTestProject PRIVATE MyMathLib gtest_main)
target_include_directories(MathTestProject PRIVATE MyMathLib)

include(GoogleTest)
gtest_discover_tests(MathTestProject)