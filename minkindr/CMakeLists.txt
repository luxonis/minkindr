cmake_minimum_required (VERSION 2.8)

if(NOT WIN32)
    set(HUNTER_CONFIGURATION_TYPES "Release" CACHE STRING "Hunter dependencies list of build configurations")
endif()
set(HUNTER_STATUS_DEBUG ON)

include("HunterGate.cmake")
HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.23.322.tar.gz"
    SHA1 "cb0ea1f74f4a2c49a807de34885743495fccccbe"
    LOCAL # Local config for dependencies
)

project(kindr)

add_definitions(-std=c++14)

hunter_add_package(Eigen)

find_package(Eigen3 CONFIG REQUIRED)

include_directories(include)
include_directories(${EIGEN3_INCLUDE_DIR})


install(DIRECTORY include/kindr DESTINATION include)

export(PACKAGE kindr)

# Create variable for the local build tree
get_property(kindr_include_dirs DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY INCLUDE_DIRECTORIES)

# Configure config file for local build tree
configure_file(kindrConfig.cmake.in
  "${PROJECT_BINARY_DIR}/kindrConfig.cmake" @ONLY)

# 2- installation build #

# Change the include location for the case of an install location
set(kindr_include_dirs ${CMAKE_INSTALL_PREFIX}/include ${EIGEN3_INCLUDE_DIR})

message("include_dirs ${kindr_include_dirs}")

# We put the generated file for installation in a different repository (i.e., ./CMakeFiles/)
configure_file(kindrConfig.cmake.in
  "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/kindrConfig.cmake" @ONLY)

install(FILES
  "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/kindrConfig.cmake"
  DESTINATION share/kindr/cmake COMPONENT dev)
 