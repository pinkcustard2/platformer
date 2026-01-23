cmake_minimum_required(VERSION 3.16)
project(platformer)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Set output folders
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# ====================
# GLFW
# ====================
include(FetchContent)

FetchContent_Declare(
        glfw
        GIT_REPOSITORY https://github.com/glfw/glfw.git
        GIT_TAG        3.3.10
)

FetchContent_MakeAvailable(glfw)

# ====================
# GLAD
# ====================
add_library(glad STATIC external/glad/src/glad.c)
target_include_directories(glad PUBLIC external/glad/include)

# ====================
# Main executable
# ====================
add_executable(platformer src/Main.cpp)

# Link GLAD and GLFW
target_link_libraries(platformer PRIVATE glad glfw)

# On Windows, link OpenGL system library
if (WIN32)
    target_link_libraries(platformer PRIVATE opengl32)
elseif(APPLE)
    find_library(COCOA Cocoa)
    find_library(IOKIT IOKit)
    find_library(COREVIDEO CoreVideo)
    find_library(OpenGL OpenGL)
    target_link_libraries(platformer PRIVATE ${COCOA} ${IOKIT} ${COREVIDEO} ${OpenGL})
else()
    find_package(OpenGL REQUIRED)
    target_link_libraries(platformer PRIVATE OpenGL::GL)
endif()

# ====================
# Include directories
# ====================
target_include_directories(platformer PRIVATE external/glad/include)
