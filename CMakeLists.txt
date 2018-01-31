cmake_minimum_required(VERSION 2.8)

project(deeprob_cv)
set(DEEPROB_CV_VERSION_MAJOR 0)
set(DEEPROB_CV_VERSION_MINOR 1)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC -march=native -fopenmp")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC -march=native -fopenmp")
enable_testing()

option(BUILD_SHARED "Build shared libraries" OFF)
if (BUILD_SHARED)
    set(DEEPROB_LIBRARY_TYPE SHARED)
else(BUILD_SHARED)
    set(DEEPROB_LIBRARY_TYPE STATIC)
endif(BUILD_SHARED)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")

find_package(SSE)
if (${SSE2_FOUND})
    set(USE_SSE2 1)
endif(${SSE2_FOUND})
if (${SSE3_FOUND})
    set(USE_SSE3 1)
endif(${SSE3_FOUND})
if (${SSSE3_FOUND})
    set(USE_SSSE3 1)
endif(${SSSE3_FOUND})
if (${SSE4_1_FOUND})
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -msse4.1")
    set(USE_SSE4_1 1)
endif(${SSE4_1_FOUND})
if (${SSE4_2_FOUND})
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -msse4.2")
    set(USE_SSE4_2 1)
endif()

configure_file("${CMAKE_SOURCE_DIR}/cmake/config/deeprob_cv_config.h.in"
               "${CMAKE_SOURCE_DIR}/config/include/deeprob_cv_config.h")

find_package(Boost 1.40 COMPONENTS date_time system filesystem thread program_options random REQUIRED)

#find_package(Eigen REQUIRED)
message(STATUS "Eigen Found: " ${EIGEN_VERSION})
if (${EIGEN_FOUND})
    set(USE_EIGEN 1)
endif(${EIGEN_FOUND})

include_directories(${EIGEN_INCLUDE_DIRS})
include_directories(${CMAKE_SOURCE_DIR})
include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})
link_directories(${CMAKE_BINARY_DIR})

add_subdirectory(stereo)

install(DIRECTORY include DESTINATION include/${CMAKE_PROJECT_NAME})
