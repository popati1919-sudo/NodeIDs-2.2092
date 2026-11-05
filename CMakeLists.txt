cmake_minimum_required(VERSION 3.21)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_VISIBILITY_PRESET hidden)
project(NodeIDs VERSION 1.0.0)

if (NOT DEFINED ENV{GEODE_SDK})
    message(FATAL_ERROR "Unable to find Geode SDK! Define GEODE_SDK.")
endif()

# Geode 6.0.0-alpha.1 ships with 2.2091 bindings. GD 2.2092 is a
# point update; expose the requested target version to the SDK's
# compatibility check while retaining the alpha.1 SDK and bindings.
set(GEODE_GD_VERSION "2.2092" CACHE INTERNAL "Geode GD Version" FORCE)
set(GEODE_COMP_GD_VERSION "22092" CACHE INTERNAL "Geode Comparable GD Version" FORCE)

file(GLOB_RECURSE SOURCES CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/upstream/src/*.cpp")
add_library(${PROJECT_NAME} SHARED ${SOURCES})
target_include_directories(${PROJECT_NAME} PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/upstream/src")
add_subdirectory($ENV{GEODE_SDK} ${CMAKE_CURRENT_BINARY_DIR}/geode)
setup_geode_mod(${PROJECT_NAME})
