cmake_minimum_required(VERSION 3.4)

set(CMAKE_CXX_STANDART 11)
set(CMAKE_CXX_STANDART_REQUIRED ON)
project(formatter_lib)
add_library(formatter STATIC formatter.cpp)
target_include_directories(formatter PUBLIC ${CMAKE_CURRENT_SOURCES_DIR})
