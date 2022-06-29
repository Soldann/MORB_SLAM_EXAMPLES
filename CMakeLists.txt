cmake_minimum_required(VERSION 3.16)

PROJECT(main)
SET(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} "-std=c++14 -pthread")
add_definitions(-DCOMPILEDWITHC11)

#FIND_LIBRARY(ORB_SLAM3 ${CMAKE_SOURCE_DIR}/libORB_SLAM3.so REQUIRED)

find_package(OpenCV 4.4)
   if(NOT OpenCV_FOUND)
	         message(FATAL_ERROR "OpenCV > 4.4 not found.")
   endif()

MESSAGE("OPENCV VERSION:")
MESSAGE(${OpenCV_VERSION})

find_package(Eigen3 3.1.0 REQUIRED)
find_package(Pangolin REQUIRED)
find_package(realsense2)
find_package(MORB_SLAM REQUIRED)

include_directories(
	${EIGEN3_INCLUDE_DIR}
	${Pangolin_INCLUDE_DIRS}
	)

ADD_EXECUTABLE(main ${CMAKE_SOURCE_DIR}/rgbd_inertial_realsense_D435i.cc)
TARGET_LINK_LIBRARIES(main 
	MORB_SLAM::MORB_SLAM 
	${OpenCV_LIBS}
	${EIGEN3_LIBS}
	${Pangolin_LIBRARIES}
	#${CMAKE_SOURCE_DIR}/libDBoW2.so
	#${CMAKE_SOURCE_DIR}/libg2o.so
	-lboost_serialization
	-lcrypto
	)
