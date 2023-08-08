cmake_minimum_required (VERSION 3.4)

if(NOT WIN32)
    set(HUNTER_CONFIGURATION_TYPES "Release" CACHE STRING "Hunter dependencies list of build configurations")
endif()

include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.23.322.tar.gz"
    SHA1 "cb0ea1f74f4a2c49a807de34885743495fccccbe"
    LOCAL # Local config for dependencies
)

project(kindr VERSION 1.0.0)

add_definitions(-std=c++14)

add_subdirectory(minkindr)