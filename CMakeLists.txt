cmake_minimum_required(VERSION 3.30)
project(vulkan_renderer)

set(CMAKE_CXX_STANDARD 20)

include(FetchContent)
FETCHCONTENT_DECLARE(GLFW GIT_REPOSITORY https://github.com/glfw/glfw.git GIT_TAG 3.4)
FETCHCONTENT_MAKEAVAILABLE(GLFW)
FETCHCONTENT_DECLARE(GLM GIT_REPOSITORY https://github.com/g-truc/glm.git GIT_TAG 1.0.1)
FETCHCONTENT_MAKEAVAILABLE(GLM)

find_package(Vulkan REQUIRED)
if(Vulkan_FOUND)
    include_directories(${Vulkan_INCLUDE_DIRS})
    link_directories(${Vulkan_LIBRARY_DIRS})
endif()

file(GLOB_RECURSE SOURCE_FILES
    ${CMAKE_SOURCE_DIR}/src/*.c
    ${CMAKE_SOURCE_DIR}/src/*.cpp)

file(GLOB_RECURSE HEADER_FILES
        ${CMAKE_SOURCE_DIR}/src/*.h
        ${CMAKE_SOURCE_DIR}/src/*.hpp)

add_executable(vulkan_renderer main.cpp ${HEADER_FILES} ${SOURCE_FILES})

add_custom_command(TARGET vulkan_renderer POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
    ${PROJECT_SOURCE_DIR}/resources
    ${PROJECT_BINARY_DIR}/resources
    COMMENT "Copying resources into binary directory")

target_link_libraries(vulkan_renderer glfw glm ${Vulkan_LIBRARIES})