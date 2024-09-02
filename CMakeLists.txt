cmake_minimum_required(VERSION 3.20)

project(
    vulkan-renderer
    VERSION 1.0
    LANGUAGES CXX
)

find_package(Vulkan REQUIRED)

include(FetchContent)
include(cmake/glslc.cmake)

FetchContent_Declare(
    glfw
    GIT_REPOSITORY https://github.com/glfw/glfw.git
    GIT_TAG 3.4
)

FetchContent_Declare(
    glm
    GIT_REPOSITORY https://github.com/g-truc/glm.git
    GIT_TAG 1.0.1
)

FetchContent_MakeAvailable(glfw glm)

set(
    SOURCES
    "src/main.cpp"
)

set(GLSL_SOURCE_DIR "${CMAKE_SOURCE_DIR}/src/shaders")
set(
    GLSL_SOURCES
    "${GLSL_SOURCE_DIR}/triangle.frag.glsl"
    "${GLSL_SOURCE_DIR}/triangle.vert.glsl"
)

add_executable(
    ${PROJECT_NAME}
    ${SOURCES}
)

set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 17)

target_add_shaders(${PROJECT_NAME} "${GLSL_SOURCES}")

target_include_directories(
    ${PROJECT_NAME}
    PUBLIC ${Vulkan_INCLUDE_DIRS}
)

target_link_libraries(
    ${PROJECT_NAME}
    ${Vulkan_LIBRARIES}
    glfw
    glm::glm
)