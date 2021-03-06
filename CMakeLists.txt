cmake_minimum_required(VERSION 2.8)

# build type defaults to debug
if(NOT DEFINED CMAKE_BUILD_TYPE)
	set(CMAKE_BUILD_TYPE Debug CACHE STRING "Choose the type of build, options are: None(CMAKE_CXX_FLAGS or CMAKE_C_FLAGS used) Debug Release RelWithDebInfo MinSizeRel.")
endif(NOT DEFINED CMAKE_BUILD_TYPE)
# set default compiler flags to enable C++14
if(NOT DEFINED CMAKE_CXX_FLAGS)
	set(CMAKE_CXX_FLAGS "-std=c++14 -march=native" CACHE STRING "Flags used by the compiler during all build types.")
endif(NOT DEFINED CMAKE_CXX_FLAGS)
# enable some optimization even in debug build
if(NOT DEFINED CMAKE_CXX_FLAGS_DEBUG)
	set(CMAKE_CXX_FLAGS_DEBUG "-g -Og" CACHE STRING "Flags used by the compiler during debug builds.")
endif(NOT DEFINED CMAKE_CXX_FLAGS_DEBUG)

project(SIMDSetOperations CXX)


find_package(OpenMP REQUIRED)

configure_file ("${CMAKE_CURRENT_SOURCE_DIR}/projectconfig.h.in" "${CMAKE_CURRENT_BINARY_DIR}/projectconfig.h")

include_directories(${CMAKE_CURRENT_BINARY_DIR})

add_definitions(${OpenMP_CXX_FLAGS})

add_executable(genlists genLists.cpp)

add_executable(set_intersection set_intersection.cpp)
set_target_properties(set_intersection PROPERTIES LINK_FLAGS ${OpenMP_CXX_FLAGS})

add_executable(set_union set_union.cpp)
set_target_properties(set_union PROPERTIES LINK_FLAGS ${OpenMP_CXX_FLAGS})
