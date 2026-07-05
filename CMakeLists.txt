cmake_minimum_required(VERSION 3.16)
project(disomaster-qt6 VERSION 1.0.0 LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_TESTS "Build test executables" OFF)

add_compile_definitions(QT_NO_KEYWORDS)

add_subdirectory(libdisomaster)

if(BUILD_TESTS)
    add_subdirectory(tests)
endif()