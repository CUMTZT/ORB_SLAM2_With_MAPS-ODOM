cmake_minimum_required(VERSION 3.0.2)
project(ORB_SLAM2_With_Maps-Odom)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  message_generation
  actionlib_msgs
  geometry_msgs
  move_base_msgs
  nav_msgs
  roscpp
  std_msgs
  tf
)

find_package(Boost COMPONENTS filesystem system REQUIRED)
find_package(OpenCV 3.0 QUIET)
find_package(octomap REQUIRED)
find_package(Eigen3 3.1.0 REQUIRED)
find_package(Pangolin REQUIRED)
find_package(PCL 1.7 REQUIRED)

include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

include_directories(
${OCTOMAP_INCLUDE_DIRS}
${PROJECT_SOURCE_DIR}
${PROJECT_SOURCE_DIR}/include
${Pangolin_INCLUDE_DIRS}
${catkin_INCLUDE_DIRS}
${Boost_INCLUDE_DIRS}
)

set(LIBS 
${OpenCV_LIBS} 
${EIGEN3_LIBS}
${Pangolin_LIBRARIES}
${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/lib/libDBoW2.so
${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o.so
${PROJECT_SOURCE_DIR}/lib/libORB_SLAM2.so
${OCTOMAP_LIBRARIES}
${Boost_LIBRARIES} 
)


add_executable(${PROJECT_NAME}
src/ros_rgbd.cc
src/Publisher/pointCloudPubliser.cpp
)
add_dependencies(${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(${PROJECT_NAME}
  ${catkin_LIBRARIES}
  ${LIBS}
)
