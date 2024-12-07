cmake_minimum_required(VERSION 3.25)

project(cpp-template VERSION 0.1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

if(NOT DEFINED CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE "Debug")
endif()

add_compile_options(
  -Wall -Wextra -Wpedantic
)

file(GLOB_RECURSE HEADERS "include/*/*.hpp" "include/*.hpp")
message(STATUS "Found headers: ${HEADERS}")
file(GLOB_RECURSE SOURCES "src/*/*.cpp" "src/*.cpp")
message(STATUS "Found sources: ${SOURCES}")

add_executable(${PROJECT_NAME} ${HEADERS} ${SOURCES})
target_include_directories(${PROJECT_NAME} PRIVATE include)

if(CMAKE_BUILD_TYPE STREQUAL "Debug")
  target_compile_definitions(${PROJECT_NAME} PRIVATE DEBUG_MODE=1)
  target_compile_options(${PROJECT_NAME} PRIVATE -Og -g3 -DDEBUG)
  target_link_options(${PROJECT_NAME} PRIVATE -fsanitize=address)
elseif(CMAKE_BUILD_TYPE STREQUAL "Release")
  target_compile_options(${PROJECT_NAME} PRIVATE -O3 -flto -DNDEBUG)
  target_link_options(${PROJECT_NAME} PRIVATE -flto)
endif()
