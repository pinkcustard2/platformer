cmake_minimum_required(VERSION 3.16)
project(platformer LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(FetchContent)

# =====================
# GLFW
# =====================
FetchContent_Declare(
        glfw
        GIT_REPOSITORY https://github.com/glfw/glfw.git
        GIT_TAG 3.3.10
)
FetchContent_MakeAvailable(glfw)

# =====================
# GLAD (local, generated)
# =====================
add_library(glad STATIC
        external/glad/src/glad.c
)

target_include_directories(glad PUBLIC
        external/glad/include
)

# =====================
# Executable
# =====================
add_executable(platformer src/main.cpp)

target_link_libraries(platformer PRIVATE glfw glad)

# =====================
# OpenGL
# =====================
if (APPLE)
    target_link_libraries(platformer PRIVATE "-framework OpenGL")
elseif (UNIX)
    find_package(OpenGL REQUIRED)
    target_link_libraries(platformer PRIVATE OpenGL::GL)
endif()
