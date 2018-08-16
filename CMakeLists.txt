cmake_minimum_required(VERSION 2.8.3)
project(demo_lidar)

set(CUDA_USE_STATIC_CUDA_RUNTIME OFF)
set(CMAKE_BUILD_TYPE Release)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  sensor_msgs
  std_msgs
  cv_bridge
  pcl_ros
)

find_package(Eigen3 REQUIRED)
find_package(PCL REQUIRED)
set(OpenCV_DIR $ENV{OPENCV2_INSTALL_DIR})
find_package(OpenCV 2 REQUIRED)

include_directories(
  ${catkin_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIR}
  ${PCL_INCLUDE_DIRS}
)

LIST(APPEND LINK_LIBS
  ${catkin_LIBRARIES}
  ${PCL_LIBRARIES}
  ${OpenCV_LIBRARIES}
)

catkin_package(
  CATKIN_DEPENDS roscpp std_msgs sensor_msgs cv_bridge pcl_ros
  DEPENDS EIGEN3 PCL OpenCV
)

SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
include_directories("${PROJECT_SOURCE_DIR}/include")

add_subdirectory(isamlib)
link_libraries(isamlib)


add_executable(featureTracking_lidar src/featureTracking.cpp)
add_executable(visualOdometry_lidar src/visualOdometry.cpp)
add_executable(bundleAdjust_lidar src/bundleAdjust.cpp)
add_executable(processDepthmap_lidar src/processDepthmap.cpp)
add_executable(stackDepthPoint_lidar src/stackDepthPoint.cpp)
add_executable(transformMaintenance_lidar src/transformMaintenance.cpp)
add_executable(registerPointCloud_lidar src/registerPointCloud.cpp)

target_link_libraries(featureTracking_lidar ${LINK_LIBS})
target_link_libraries(visualOdometry_lidar  ${LINK_LIBS})
target_link_libraries(bundleAdjust_lidar  ${LINK_LIBS})
target_link_libraries(processDepthmap_lidar  ${LINK_LIBS})
target_link_libraries(stackDepthPoint_lidar  ${LINK_LIBS})
target_link_libraries(transformMaintenance_lidar  ${LINK_LIBS})
target_link_libraries(registerPointCloud_lidar  ${LINK_LIBS})
