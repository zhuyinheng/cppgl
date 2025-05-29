cmake_minimum_required(VERSION 3.16)

PROJECT(CppGL
    VERSION 1.9
    DESCRIPTION "OpenGL framework for use in teaching and research with focus on simplicity and reusability."
    LANGUAGES CXX
)

if (NOT UNIX AND NOT WIN32)
	message(FATAL_ERROR "Unsupported platform!")
endif()

# ---------------------------------------------------------------------
# cmake options

option(CPPGL_BUILD_EXAMPLES "" OFF)

# ---------------------------------------------------------------------
# submodules

set(BUILD_SHARED_LIBS OFF CACHE BOOL "" FORCE)
set(BUILD_STATIC_LIBS ON CACHE BOOL "" FORCE)

set(glew-cmake_BUILD_STATIC ON CACHE BOOL "" FORCE)
set(glew-cmake_BUILD_SHARED OFF CACHE BOOL "" FORCE)
add_subdirectory(submodules/glew-cmake)

set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL " " FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL " " FORCE)
set(GLFW_BUILD_DOCS OFF CACHE BOOL " " FORCE)
set(GLFW_BUILD_INSTALL OFF CACHE BOOL " " FORCE)
add_subdirectory(submodules/glfw)

# assimp
set(ASSIMP_BUILD_ASSIMP_TOOLS OFF CACHE BOOL "" FORCE)
set(ASSIMP_BUILD_TESTS OFF CACHE BOOL "" FORCE)
add_subdirectory(submodules/assimp)

# ---------------------------------------------------------------------
# compiler options

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_STANDARD 17)

# ensure to use included deps
set(CMAKE_INCLUDE_DIRECTORIES_BEFORE ON)
set(CMAKE_LINK_DIRECTORIES_BEFORE ON)

if(UNIX)
    add_compile_options(-pthread -march=native)
    if(APPLE)
        # macOS specific OpenGL framework configuration
        find_library(OPENGL_FRAMEWORK OpenGL REQUIRED)
        find_library(COCOA_FRAMEWORK Cocoa REQUIRED)
        find_library(IOKIT_FRAMEWORK IOKit REQUIRED)
        find_library(COREFOUNDATION_FRAMEWORK CoreFoundation REQUIRED)
        find_library(QUARTZCORE_FRAMEWORK QuartzCore REQUIRED)
        
        include_directories(
            ${OPENGL_FRAMEWORK}/Headers
            /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/System/Library/Frameworks/OpenGL.framework/Headers
        )
        
        add_definitions(-D__APPLE__)
        # On macOS, filesystem is part of the standard library
        set(CMAKE_CXX_STANDARD 17)
        set(CMAKE_CXX_STANDARD_REQUIRED ON)
        set(CMAKE_CXX_EXTENSIONS OFF)
    endif()
endif()

# Link stdc++fs only on Linux
if(UNIX AND NOT APPLE)
    target_link_libraries(cppgl PUBLIC stdc++fs)
endif()

# ---------------------------------------------------------------------
# traverse source tree

add_subdirectory(src)

if (CPPGL_BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()

# macOS framework linking (after cppgl target is defined)
if(APPLE)
    target_link_libraries(cppgl PUBLIC
        ${OPENGL_FRAMEWORK}
        ${COCOA_FRAMEWORK}
        ${IOKIT_FRAMEWORK}
        ${COREFOUNDATION_FRAMEWORK}
        ${QUARTZCORE_FRAMEWORK}
    )
endif()
