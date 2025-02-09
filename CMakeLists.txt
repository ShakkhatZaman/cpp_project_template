cmake_minimum_required(VERSION 3.14)

project(template)

# Needed for the clangd language server
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_CXX_STANDARD 17)

option(RELEASE_BUILD "Make it a release build" OFF)
option(DEBUGGING_FEATURES "Have debugging features in the build" ON)

# Prevents compiling unnecessary binaries
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set(GLFW_INSTALL OFF CACHE BOOL "" FORCE)
set(BUILD_SHARED_LIBS OFF CACHE BOOL "" FORCE)

# Detect and add the subdirectories for all libraries in "lib"
### Each subdirectory MUST CONTAIN a CMakeLists.txt file.
file(GLOB children CONFIGURE_DEPENDS RELATIVE ${CMAKE_SOURCE_DIR} "${CMAKE_SOURCE_DIR}/lib/*")
foreach(child ${children})
    if(IS_DIRECTORY ${child})
        add_subdirectory(${child})
    endif()
endforeach()

# find and set all source files and libraries
file(GLOB_RECURSE SOURCE_FILES CONFIGURE_DEPENDS "${CMAKE_SOURCE_DIR}/src/*.cpp")
set(LINKING_LIBRARIES
    glfw
    glad
    glm
    imgui
    stb_image
)

add_executable(${PROJECT_NAME})

target_compile_definitions(${PROJECT_NAME} PUBLIC GLFW_INCLUDE_NONE)

if(RELEASE_BUILD)
    set(CMAKE_BUILD_TYPE Release)
    target_compile_definitions(${PROJECT_NAME} PRIVATE RESOURCES_DIR="./res")
    message(NOTICE "##--> Configuring Makefiles for a RELEASE build. <--##")
else()
    set(CMAKE_BUILD_TYPE Debug)
    target_compile_definitions(${PROJECT_NAME} PRIVATE RESOURCES_DIR="${CMAKE_SOURCE_DIR}/res")
    message(NOTICE "##--> Configuring Makefiles for a DEBUG build. <--##")
endif()

if(DEBUGGING_FEATURES)
    target_compile_definitions(${PROJECT_NAME} PRIVATE __DEBUG__)
    message(NOTICE "Build will contain Debugging features.")
else()
    message(NOTICE "Build will NOT contain Debugging features.")
endif()

target_sources(${PROJECT_NAME} PRIVATE ${SOURCE_FILES})

target_include_directories(${PROJECT_NAME} PUBLIC "${CMAKE_SOURCE_DIR}/src/include/")

target_link_libraries(${PROJECT_NAME} PRIVATE ${LINKING_LIBRARIES})

# unset so the variables aren't cached
unset(RELEASE_BUILD CACHE)
unset(DEBUGGING_FEATURES CACHE)
