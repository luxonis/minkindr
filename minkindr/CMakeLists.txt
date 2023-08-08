cmake_minimum_required (VERSION 3.4)

hunter_add_package(Eigen)

find_package(Eigen3 CONFIG REQUIRED)
include(GNUInstallDirs)
include(CMakePackageConfigHelpers)
include(CMakeDependentOption)


message("Project name is -> ${PROJECT_NAME}")
add_library(${PROJECT_NAME} INTERFACE)
add_library (${PROJECT_NAME}::${PROJECT_NAME}  ALIAS ${PROJECT_NAME})

set(CMAKEPACKAGE_INSTALL_DIR
    "${CMAKE_INSTALL_DATADIR}/${PROJECT_NAME}/cmake"
    CACHE PATH "The directory relative to CMAKE_INSTALL_PREFIX where ${PACKAGE_NAME}Config.cmake is installed"
    )

target_include_directories(${PROJECT_NAME}  INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

install(TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}Targets
    INCLUDES DESTINATION include
)

install(DIRECTORY include/ 
    DESTINATION include
)

configure_package_config_file (
  ${CMAKE_CURRENT_SOURCE_DIR}/${PROJECT_NAME}Config.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
  INSTALL_DESTINATION ${CMAKEPACKAGE_INSTALL_DIR}
)

install(EXPORT ${PROJECT_NAME}Targets 
    NAMESPACE ${PROJECT_NAME}:: 
    DESTINATION ${CMAKEPACKAGE_INSTALL_DIR})

# Optionally, create a ConfigVersion file
write_basic_package_version_file(${PROJECT_NAME}ConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
         DESTINATION ${CMAKEPACKAGE_INSTALL_DIR})

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake
         DESTINATION ${CMAKEPACKAGE_INSTALL_DIR})
     
export (TARGETS ${PROJECT_NAME} NAMESPACE ${PROJECT_NAME}:: FILE ${PROJECT_NAME}Targets.cmake)
export (PACKAGE ${PROJECT_NAME})
# install(EXPORT ${PROJECT_NAME}Targets
#     FILE ${PROJECT_NAME}Config.cmake
#     DESTINATION "${CMAKE_INSTALL_PREFIX}/lib/cmake/${PROJECT_NAME}"
# )

# install(DIRECTORY include/ 
#     DESTINATION include
# )
# message("CMAKE_INSTALL_PREFIX is  -> ${CMAKE_INSTALL_PREFIX}  ")
# message("CMAKE_INSTALL_LIBDIR is  -> ${CMAKE_INSTALL_LIBDIR}  ")

# export(TARGETS ${PROJECT_NAME}
#        NAMESPACE ${PROJECT_NAME}::
#        FILE "${CMAKE_INSTALL_PREFIX}/${PROJECT_NAME}Config.cmake"
# )

# export(PACKAGE ${PROJECT_NAME})
# ----------------------------------------------------------------

# include_directories(include)
# include_directories(${EIGEN3_INCLUDE_DIR})





# install(DIRECTORY include/kindr DESTINATION include)

# export(PACKAGE kindr)

# # Create variable for the local build tree
# get_property(kindr_include_dirs DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY INCLUDE_DIRECTORIES)

# # Configure config file for local build tree
# configure_file(kindrConfig.cmake.in
#   "${PROJECT_BINARY_DIR}/kindrConfig.cmake" @ONLY)

# # 2- installation build #

# # Change the include location for the case of an install location
# set(kindr_include_dirs ${CMAKE_INSTALL_PREFIX}/include ${EIGEN3_INCLUDE_DIR})

# message("include_dirs ${kindr_include_dirs}")
# message("Conifg dir ${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}")
# message("CMAKE_INSTALL_PREFIX dir ${CMAKE_INSTALL_PREFIX}")

# # We put the generated file for installation in a different repository (i.e., ./CMakeFiles/)
# configure_file(kindrConfig.cmake.in
#   "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/kindrConfig.cmake" @ONLY)

# install(FILES
#   "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/kindrConfig.cmake"
#   DESTINATION share/kindr/cmake COMPONENT dev)
 