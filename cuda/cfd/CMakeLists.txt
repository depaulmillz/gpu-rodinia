cmake_minimum_required(VERSION 3.10)

project(CFD LANGUAGES CUDA)

message("BUILD TYPE: ${CMAKE_BUILD_TYPE}")

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

message("SDK ROOT DIR ${CUDA_PATH} ${CUDA_TOOLKIT_ROOT_DIR}")

add_executable(euler3d euler3d.cu) 
add_executable(euler3d_double euler3d_double.cu)  
add_executable(pre_euler3d pre_euler3d.cu)  
add_executable(pre_euler3d_double pre_euler3d_double.cu)

target_include_directories(euler3d PUBLIC /usr/local/cuda/samples/common/inc)
target_include_directories(euler3d_double PUBLIC /usr/local/cuda/samples/common/inc)
target_include_directories(pre_euler3d PUBLIC /usr/local/cuda/samples/common/inc)
target_include_directories(pre_euler3d_double PUBLIC /usr/local/cuda/samples/common/inc)
