cmake_minimum_required(VERSION 3.30)
project(vulkan_renderer)

include(FetchContent)
FETCHCONTENT_DECLARE(GLFW GIT_REPOSITORY https://github.com/glfw/glfw.git GIT_TAG 3.4)
FETCHCONTENT_MAKEAVAILABLE(GLFW)

find_package(Vulkan REQUIRED)
if(Vulkan_FOUND)
    include_directories(${Vulkan_INCLUDE_DIRS})
    link_directories(${Vulkan_LIBRARY_DIRS})
endif()

set(CMAKE_CXX_STANDARD 20)

add_executable(vulkan_renderer main.cpp)

target_link_libraries(vulkan_renderer glfw ${Vulkan_LIBRARIES})