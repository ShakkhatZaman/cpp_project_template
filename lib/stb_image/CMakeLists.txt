cmake_minimum_required(VERSION 3.14)

project(stb_image)

add_library(stb_image STATIC "${CMAKE_CURRENT_SOURCE_DIR}/stb_image.cpp")
