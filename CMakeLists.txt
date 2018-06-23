cmake_minimum_required (VERSION 3.0)
cmake_policy(VERSION 3.0)

project (opengl_project)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/modules/")

list(APPEND CMAKE_CXX_FLAGS "-std=c++11")

# find the required packages
find_package(GLM REQUIRED)
message(STATUS "GLM included at ${GLM_INCLUDE_DIR}")
find_package(GLFW3 REQUIRED)
message(STATUS "Found GLFW3 in ${GLFW3_INCLUDE_DIR}")
find_package(ASSIMP REQUIRED)
message(STATUS "Found ASSIMP in ${ASSIMP_INCLUDE_DIR}")


set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -Wall")
find_package(OpenGL REQUIRED)
add_definitions(${OPENGL_DEFINITIONS})
find_package(X11 REQUIRED)
# note that the order is important for setting the libs
# use pkg-config --libs $(pkg-config --print-requires --print-requires-private glfw3) in a terminal to confirm
set (LIBS ${GLFW3_LIBRARY} X11 Xrandr Xinerama Xi Xxf86vm Xcursor GL dl pthread ${ASSIMP_LIBRARY})
set (CMAKE_CXX_LINK_EXECUTABLE "${CMAKE_CXX_LINK_EXECUTABLE} -ldl")


include_directories(
  includes/
  .
)

add_library(GLAD "src/glad/glad.c")
set(LIBS ${LIBS} GLAD)

add_library(STB_IMAGE "src/stb_image.cpp")
set(LIBS ${LIBS} STB_IMAGE)

file(GLOB src_files
    "src/*.cpp"
)

file(GLOB common_files
    "common/*.hpp"
    "common/*.cpp"
)

add_executable(opengl_project
         ${src_files}
         ${common_files}
)

target_link_libraries(opengl_project ${LIBS})
