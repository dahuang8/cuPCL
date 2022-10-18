cmake_minimum_required(VERSION 3.18)
project(cuda_seg)

set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

include(setup_cuda)

find_package(CUDA REQUIRED)
find_package(PCL REQUIRED)

add_library(cuda_segmenatation_lib SHARED IMPORTED )
set_target_properties(cuda_segmenatation_lib PROPERTIES IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/lib/libcudasegmentation.so)

cuda_add_executable(cuda_seg main.cpp OPTIONS -arch=sm_35)
target_include_directories(cuda_seg PUBLIC ${PCL_INCLUDE_DIRS})
target_link_directories(cuda_seg PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/lib)
target_link_libraries(cuda_seg ${PCL_LIBRARIES} cuda_segmenatation_lib)
set_target_properties(cuda_seg PROPERTIES CUDA_SEPARABLE_COMPILATION ON)