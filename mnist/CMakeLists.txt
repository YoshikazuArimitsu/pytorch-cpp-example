cmake_minimum_required(VERSION 3.1 FATAL_ERROR)
project(mnist)

# for Amazon DeepLearning AMI
# set(Torch_DIR "$ENV{HOME}/anaconda3/envs/pytorch_p36/lib/python3.6/site-packages/torch/share/cmake/Torch")
set(Caffe2_DIR "../libtorch/share/cmake/Caffe2")
set(Torch_DIR "../libtorch/share/cmake/Torch")

find_package(Torch REQUIRED)

option(DOWNLOAD_MNIST "Download the MNIST dataset from the internet" ON)
if (DOWNLOAD_MNIST)
  message(STATUS "Downloading MNIST dataset")
  execute_process(
    COMMAND python ${CMAKE_CURRENT_LIST_DIR}/download_mnist.py
      -d ${CMAKE_BINARY_DIR}/data
    ERROR_VARIABLE DOWNLOAD_ERROR)
  if (DOWNLOAD_ERROR)
    message(FATAL_ERROR "Error downloading MNIST dataset: ${DOWNLOAD_ERROR}")
  endif()
endif()


set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${TORCH_CXX_FLAGS}")

add_executable(mnist mnist.cpp)
target_compile_features(mnist PUBLIC cxx_range_for)
target_link_libraries(mnist ${TORCH_LIBRARIES})
set_property(TARGET mnist PROPERTY CXX_STANDARD 14)
