cmake_minimum_required(VERSION 3.14)

project(imgui)

add_library(imgui STATIC)

set(IMGUI_DIR "${CMAKE_CURRENT_SOURCE_DIR}/imgui")

file(GLOB IMGUI_SOURCES CONFIGURE_DEPENDS "${IMGUI_DIR}/*.cpp")

list(APPEND IMGUI_SOURCES
    "${IMGUI_DIR}/backends/imgui_impl_opengl3.cpp"
    "${IMGUI_DIR}/backends/imgui_impl_glfw.cpp"
)

target_sources(imgui PRIVATE ${IMGUI_SOURCES})

target_include_directories(imgui PUBLIC "${IMGUI_DIR}/" PUBLIC "${IMGUI_DIR}/backends/")

target_link_libraries(imgui PUBLIC glad glfw)
