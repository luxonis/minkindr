cmake_minimum_required (VERSION 3.4)

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
message("Conifg dir ${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}")
message("CMAKE_INSTALL_PREFIX dir ${CMAKE_INSTALL_PREFIX}")

# We put the generated file for installation in a different repository (i.e., ./CMakeFiles/)
configure_file(kindrConfig.cmake.in
  "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/kindrConfig.cmake" @ONLY)

install(FILES
  "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/kindrConfig.cmake"
  DESTINATION share/kindr/cmake COMPONENT dev)
 