cmake_minimum_required(VERSION 2.8.4)

set(CMAKE_CXX_FLAGS " -std=c++11")

# define, up to you
project( libPrediction )

set(CMAKE_BUILD_TYPE Release)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake-modules)

set( CUDA_USE_STATIC_CUDA_RUNTIME OFF )

set( CMAKE_CXX_STANDARD_REQUIRED ON )

message("OPENCV")
set( OpenCV_DIR /home/user/opencv-2.4.13.2/release )
set(OpenCV_FOUND 1)
find_package( OpenCV )
find_package( OpenCV REQUIRED )

message("CUDA")
#find_package( CUDA )
find_package( CUDA QUIET REQUIRED )

#message("BOOST")
#find_package( Boost COMPONENTS filesystem system REQUIRED)

message("EIGEN")


find_package( Eigen3 REQUIRED )

#message("CAFFE")
#set( Caffe_DIR "/home/user/caffe/cmake" )
#set( Caffe_INCLUDE_DIRS "/home/user/caffe/build/include" )
#set( Caffe_LIBRARIES "/home/user/caffe/build/lib/libcaffe.so" )
#find_package( Caffe REQUIRED )

message("FINISH FIND PACKAGE")

#head file path  

set(
	CUDA_NVCC_FLAGS
	${CUDA_NVCC_FLAGS};
    -O3 
	-gencode arch=compute_53,code=sm_53
	-gencode arch=compute_62,code=sm_62
	#-gencode arch=compute_66,code=sm_66
)


include_directories(${CMAKE_BINARY_DIR})
include_directories(/usr/include)
include_directories(${EIGEN3_INCLUDE_DIR})


set( MySQL_DIR "/home/user/mysql-connector-c++-1.1.9-linux-ubuntu16.04-x86-64bit/" )
include_directories(${MySQL_DIR}/include)
link_directories(${MySQL_DIR}/lib)

#INCLUDE_DIRECTORIES(${Boost_INCLUDE_DIRS})
#LINK_DIRECTORIES(${Boost_LIBRARY_DIRS})	

link_directories(/usr/lib)

#include_directories(${Caffe_INCLUDE_DIRS})
#add_definitions(${Caffe_DEFINITIONS})


set(LIB_FILES
	libPrediction.cpp
	)
	
	
#source directory   

add_library( ${PROJECT_NAME} SHARED ${LIB_FILES} )

target_link_libraries( ${PROJECT_NAME} ${OpenCV_LIBS} nvcaffe_parser nvinfer)

#target_link_libraries( ${PROJECT_NAME} ${Caffe_LIBRARIES} ${Boost_LIBRARIES} )

target_link_libraries( ${PROJECT_NAME} mysqlcppconn)


