cmake_minimum_required(VERSION 3.18)
project(TensorRTInference VERSION 1.0.0 LANGUAGES CXX CUDA)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CUDA_STANDARD 17)
set(CMAKE_CUDA_ARCHITECTURES 86 CACHE STRING "CUDA architectures" FORCE)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

find_package(CUDAToolkit REQUIRED)
find_package(OpenCV REQUIRED)

set(TensorRT_INCLUDE_DIR "/usr/include/x86_64-linux-gnu" CACHE PATH "TensorRT include")
set(TensorRT_LIB_DIR "/usr/lib/x86_64-linux-gnu" CACHE PATH "TensorRT library")

file(GLOB_RECURSE SOURCES "src/*.cpp" "src/*.cu")
add_executable(${PROJECT_NAME} ${SOURCES})

target_include_directories(${PROJECT_NAME} PRIVATE
    src
    ${TensorRT_INCLUDE_DIR}
    ${OpenCV_INCLUDE_DIRS}
)

target_link_libraries(${PROJECT_NAME} PRIVATE
    ${OpenCV_LIBS}
    CUDA::cudart
    CUDA::cublas
    cudnn
    nvinfer
    nvinfer_plugin
    nvonnxparser
    pthread
    dl
)

target_link_directories(${PROJECT_NAME} PRIVATE ${TensorRT_LIB_DIR})
