cmake_minimum_required(VERSION 2.8.3)
project(ccmslam)

set(PERFORMANCE_MODE TRUE)

set(DEBUG_MODE 0)

IF(G2O_U14)
MESSAGE( STATUS "U14 Configuration")
add_definitions(-DU14_G2O)
ENDIF()


set (CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_FLAGS "-pthread -pipe ${CMAKE_CXX_FLAGS}")
set(CMAKE_CXX_FLAGS "-Wall ${CMAKE_CXX_FLAGS}")
set(CMAKE_CXX_FLAGS "-Wno-reorder -Wno-sign-compare -Wno-unused-function -Wno-strict-aliasing ${CMAKE_CXX_FLAGS}")
if(${KINETIC})
set(CMAKE_CXX_FLAGS "-Wno-unused-variable ${CMAKE_CXX_FLAGS}")
endif()
set(CMAKE_CXX_FLAGS "-Werror=parentheses -Werror=unused-value ${CMAKE_CXX_FLAGS}")
if(NOT ${KINETIC})
set(CMAKE_CXX_FLAGS "-Werror=unused-variable ${CMAKE_CXX_FLAGS}")
endif()

if(${PERFORMANCE_MODE})
    set(CMAKE_CXX_FLAGS "-Wfatal-errors ${CMAKE_CXX_FLAGS}")
    set(CMAKE_CXX_FLAGS "-O3 ${CMAKE_CXX_FLAGS}")
endif()

#if(NOT ${KINETIC})
#    set(CMAKE_CXX_FLAGS "-L${PROJECT_SOURCE_DIR}/thirdparty/opencv-2.4.13/build/lib/ -lopencv_nonfree ${CMAKE_CXX_FLAGS}")
#endif()

if(${DEBUG_MODE})
set(CMAKE_CXX_FLAGS "-g ${CMAKE_CXX_FLAGS}")
endif()

MESSAGE( STATUS "CMAKE_CXX_FLAGS: " ${CMAKE_CXX_FLAGS} )

#ROS_BUILD_TYPE: Set the build type. Options are (default: RelWithDebInfo):
#Debug : w/ debug symbols, w/o optimization
#Release : w/o debug symbols, w/ optimization
#RelWithDebInfo : w/ debug symbols, w/ optimization
#RelWithAsserts : w/o debug symbols, w/ optimization, w/ assertions (i.e., w/o -DNDEBUG). New in ros 1.1.
#MinSizeRel : w/o debug symbols, w/ optimization, stripped binaries
set(CMAKE_BUILD_TYPE RelWithDebInfo)
#set(CMAKE_BUILD_TYPE Debug)
if(${PERFORMANCE_MODE})
    set(CMAKE_BUILD_TYPE Release)
endif()

MESSAGE( STATUS "CMAKE_BUILD_TYPE: " ${CMAKE_BUILD_TYPE} )

set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)

if(${KINETIC})
    find_package(OpenCV REQUIRED)
endif()

find_package(catkin REQUIRED COMPONENTS roscpp cv_bridge tf ccmslam_msgs pcl_ros tf_conversions image_transport)
find_package(cmake_modules REQUIRED)

#if(${KINETIC})
find_package(Eigen3 REQUIRED 3.0)
#else()
#find_package(Eigen REQUIRED 3.0)
#endif()
#include_directories(${EIGEN_INCLUDE_DIRS})
#add_definitions(${EIGEN_DEFINITIONS})
find_package(PCL 1.3 REQUIRED)

#if(NOT ${KINETIC})
#    set(OpenCV_DIR ${PROJECT_SOURCE_DIR}/thirdparty/opencv-2.4.13/build)
#    set(OpenCV_INCLUDE_DIRS ${PROJECT_SOURCE_DIR}/thirdparty/opencv-2.4.13/build/include)
#    find_package(OpenCV 2.4.13 REQUIRED)
#endif()


# Set up external project support
#include(ExternalProject)

#ExternalProject_Add(DBoW2
#	SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/DBoW2
#	BINARY_DIR ${CMAKE_CURRENT_BINARY_DIR}/thirdparty/DBoW2
#	INSTALL_DIR ${BUILD_INSTALL_PREFIX}
#	CMAKE_ARGS "-DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE};-DBENCHMARK_ENABLE_TESTING=OFF;-DCMAKE_INSTALL_PREFIX=${BUILD_INSTALL_PREFIX}" )

add_subdirectory(thirdparty/DBoW2)
add_subdirectory(thirdparty/g2o)

catkin_package(
   INCLUDE_DIRS include
   CATKIN_DEPENDS ccmslam_msgs
   LIBRARIES ccmslam_lib
   )


message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
 MESSAGE(CATKIN_PACKAGE_INCLUDE_DESTINATION=${CATKIN_PACKAGE_INCLUDE_DESTINATION})
 message(catkin_INCLUDE_DIRS=${catkin_INCLUDE_DIRS})
 message(${CATKIN_DEVEL_PREFIX}/../build/ccm_slam/cslam/thirdparty/g2o)
   message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
message(======================================================================================)
 
include_directories(
  ${PROJECT_SOURCE_DIR}
  ${PROJECT_SOURCE_DIR}/include
  ${g2o_SOURCE_DIR}
  ${catkin_INCLUDE_DIRS}
  # Yuck for now
  ${CATKIN_DEVEL_PREFIX}/../build/ccm_slam/cslam/thirdparty/g2o
  ${PCL_INCLUDE_DIRS}
  ${Eigen_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
  ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

set(EXTERNAL_LIBRARIES
  ${catkin_LIBRARIES}
  ${PCL_LIBRARIES}
  Eigen3::Eigen
)

ADD_SUBDIRECTORY( include/cslam )

add_library(ccmslam_lib
#cpp files
  src/config.cpp
  src/CentralControl.cpp
  src/ClientHandler.cpp
  src/Communicator.cpp
  src/Converter.cc
  src/Database.cpp
  src/Frame.cpp
  src/Initializer.cpp
  src/KeyFrame.cpp
  src/LoopFinder.cpp
  src/Map.cpp
  src/MapMatcher.cpp
  src/MapMerger.cpp
  src/Mapping.cpp
  src/MapPoint.cpp
  src/Optimizer.cpp
  src/ORBextractor.cpp
  src/ORBmatcher.cpp
  src/PnPSolver.cpp
  src/Sim3Solver.cpp
  src/Tracking.cpp
  src/Viewer.cpp

#header
  include/cslam/config.h
  include/cslam/CentralControl.h
  include/cslam/ClientHandler.h
  include/cslam/Communicator.h
  include/cslam/Converter.h
  include/cslam/Database.h
  include/cslam/Datatypes.h
  include/cslam/Frame.h
  include/cslam/Initializer.h
  include/cslam/KeyFrame.h
  include/cslam/LoopFinder.h
  include/cslam/Map.h
  include/cslam/MapMatcher.h
  include/cslam/MapMerger.h
  include/cslam/Mapping.h
  include/cslam/MapPoint.h
  include/cslam/Optimizer.h
  include/cslam/ORBextractor.h
  include/cslam/ORBmatcher.h
  include/cslam/ORBVocabulary.h
  include/cslam/PnPSolver.h
  include/cslam/Sim3Solver.h
  include/cslam/Tracking.h
  include/cslam/Viewer.h
  include/cslam/estd.h
)

add_dependencies(ccmslam_lib ccmslam_msgs_generate_messages_cpp)
target_link_libraries(ccmslam_lib ${EXTERNAL_LIBRARIES} DBoW2 core types_sba types_sim3)

add_executable(ccmslamClientNode
#node
  src/client/ClientNode.cpp

#cpp files
  src/client/ClientSystem.cpp

#header
  include/cslam/client/ClientSystem.h
)
add_dependencies(ccmslamClientNode ccmslam_lib)
target_link_libraries(ccmslamClientNode ${EXTERNAL_LIBRARIES} ccmslam_lib)

add_executable(ccmslamServerNode
#node
  src/server/ServerNode.cpp

#cpp files
  src/server/ServerSystem.cpp

#header
  include/cslam/server/ServerSystem.h
)
add_dependencies(ccmslamServerNode ccmslam_lib)
target_link_libraries(ccmslamServerNode ${EXTERNAL_LIBRARIES} ccmslam_lib)
