cmake_minimum_required(VERSION 3.10)

project(Mummergpu LANGUAGES CUDA CXX)

find_package(CUDA REQUIRED)

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

string(APPEND CMAKE_CXX_FLAGS " -fopenmp -fPIC")

add_library(libmummergpu STATIC src/mummergpu_gold.cpp src/suffix-tree.cpp src/PoolMalloc.cpp src/mummergpu.cu)
target_include_directories(libmummergpu PUBLIC src)
target_link_libraries(libmummergpu cuda)

add_executable(mummergpu src/mummergpu_main.cpp)
target_include_directories(mummergpu PUBLIC src)
target_include_directories(mummergpu PUBLIC ${CUDA_TOOLKIT_ROOT_DIR}/include)


target_link_libraries(mummergpu libmummergpu)
target_link_libraries(mummergpu cuda)
