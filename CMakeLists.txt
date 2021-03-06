project(LIBCONTROL CXX)
cmake_minimum_required(VERSION 2.8)
set(CMAKE_MODULE_PATH ${LIBCONTROL_SOURCE_DIR}/cmake/Modules)

find_package(Eigen3 3.1.2)

option(LIBCONTROL_BUILD_TESTS "Build tests." OFF)
set(BUILD_SHARED_LIBS ON)

# Set g++ compiler flags if GNU C++ compiler is being used
if(CMAKE_COMPILER_IS_GNUCXX)
    execute_process(COMMAND ${CMAKE_CXX_COMPILER} -dumpversion OUTPUT_VARIABLE GCC_VERSION)
    if(GCC_VERSION VERSION_GREATER 4.7 OR GCC_VERSION VERSION_EQUAL 4.7)
        set(ENABLE_CXX11 "-std=c++11")
    elseif(GCC_VERSION VERSION_GREATER 4.3 OR GCC_VERSION VERSION_EQUAL 4.3)
        set(ENABLE_CXX11 "-std=c++0x")
    else()
        message(FATAL_ERROR "C++ compiler does not support C++11")
    endif()
    set(CMAKE_CXX_FLAGS "-Wall -Wno-unused-local-typedefs ${ENABLE_CXX11}")
else()
    set(CMAKE_CXX_FLAGS "-Wall -std=c++11")
endif()
set(CMAKE_CXX_FLAGS_DEBUG "-O2 -ggdb3")
set(CMAKE_CXX_FLAGS_RELEASE "-O2")

include_directories(${LIBCONTROL_SOURCE_DIR}/include)
include_directories(${EIGEN3_INCLUDE_DIR})

if (LIBCONTROL_BUILD_TESTS)
  enable_testing()
  include_directories(${LIBCONTROL_SOURCE_DIR}/googletest)
  include_directories(${LIBCONTROL_SOURCE_DIR}/googletest/include)
  add_subdirectory(googletest)
endif()

add_subdirectory(${LIBCONTROL_SOURCE_DIR}/include)
add_subdirectory(${LIBCONTROL_SOURCE_DIR}/source)

