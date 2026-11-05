cmake_minimum_required(VERSION 3.21)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_VISIBILITY_PRESET hidden)
project(NodeIDs VERSION 1.0.0)

if (NOT DEFINED ENV{GEODE_SDK})
    message(FATAL_ERROR "Unable to find Geode SDK! Define GEODE_SDK.")
endif()

# Geode 6.0.0-alpha.1 only provides bindings for GD 2.2091.
# Use that supported bindings set; mod.json uses "*" so Geode's
# package validator does not falsely label these as 2.2092-specific.
set(GEODE_GD_VERSION "2.2091" CACHE INTERNAL "Geode GD Version" FORCE)
set(GEODE_COMP_GD_VERSION "22091" CACHE INTERNAL "Geode Comparable GD Version" FORCE)

file(GLOB_RECURSE SOURCES CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/upstream/src/*.cpp")
add_library(${PROJECT_NAME} SHARED ${SOURCES})
target_include_directories(${PROJECT_NAME} PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/upstream/src")
add_subdirectory($ENV{GEODE_SDK} ${CMAKE_CURRENT_BINARY_DIR}/geode)
setup_geode_mod(${PROJECT_NAME})
