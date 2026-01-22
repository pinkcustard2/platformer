cmake_minimum_required(VERSION 3.16)
project(platformer LANGUAGES CXX C)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(FetchContent)

# GLFW
FetchContent_Declare(
        glfw
        GIT_REPOSITORY https://github.com/glfw/glfw.git
        GIT_TAG 3.3.9
)
FetchContent_MakeAvailable(glfw)

# GLM
FetchContent_Declare(
        glm
        GIT_REPOSITORY https://github.com/g-truc/glm.git
        GIT_TAG 1.0.1
)
FetchContent_MakeAvailable(glm)

find_package(OpenGL REQUIRED)

# GLAD (local)
add_library(glad STATIC
        external/glad/src/gl.c
)
target_include_directories(glad PUBLIC
        external/glad/include
)

add_executable(platformer
        src/Main.cpp
)

target_link_libraries(platformer
        OpenGL::GL
        glfw
        glad
        glm::glm
)
