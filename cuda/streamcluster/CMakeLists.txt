cmake_minimum_required(VERSION 3.10)

project(Streamcluster LANGUAGES CUDA CXX)

message("BUILD TYPE: ${CMAKE_BUILD_TYPE}")

string(APPEND CMAKE_CUDA_FLAGS " -use_fast_math")

if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    string(APPEND CMAKE_CUDA_FLAGS " -G -pg")
    message("CUDA FLAGS: ${CMAKE_CUDA_FLAGS}")
endif(CMAKE_BUILD_TYPE STREQUAL "Debug")

if(CMAKE_BUILD_TYPE STREQUAL "RelWithDebInfo")
    string(APPEND CMAKE_CUDA_FLAGS " -lineinfo -pg")
    message("CUDA FLAGS: ${CMAKE_CUDA_FLAGS}")
endif(CMAKE_BUILD_TYPE STREQUAL "RelWithDebInfo")

if(NOT DEFINED CUDA_ARCH)
    set(CUDA_ARCH 61)
    message("WARNING CUDA_ARCH IS BEING SET TO DEFAULT ${CUDA_ARCH}")
endif(NOT DEFINED CUDA_ARCH)

add_compile_options("$<$<CONFIG:DEBUG>:-DDEBUG>")

add_executable(sc_gpu streamcluster_cuda_cpu.cpp streamcluster_cuda.cu streamcluster_header.cu) 
target_include_directories(sc_gpu PUBLIC /usr/local/cuda/include)