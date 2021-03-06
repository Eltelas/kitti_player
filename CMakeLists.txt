cmake_minimum_required(VERSION 2.8.3)
project(kitti_player)

SET (CMAKE_BUILD_TYPE Release)
SET (CMAKE_CXX_FLAGS "-O3 -std=c++11")

find_package(catkin REQUIRED COMPONENTS
                    roscpp
                    tf
                    tf2
                    tf2_geometry_msgs
                    std_msgs
                    pcl_ros
                    geometry_msgs
                    cv_bridge
                    image_transport
                    geodesy
                    rosbag
)

find_package(PCL 1.7 REQUIRED)

generate_dynamic_reconfigure_options(cfg/kitti_player.cfg)

catkin_package(CATKIN_DEPENDS dynamic_reconfigure)

include_directories(
		${}
        ${CMAKE_CURRENT_SOURCE_DIR}/include
  		${catkin_INCLUDE_DIRS} 
  		${PCL_INCLUDE_DIRS}
)

link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

add_library(kitti src/Player.cpp src/PlayerSupport.cpp)
target_link_libraries(kitti ${catkin_LIBRARIES} ${PCL_LIBRARIES})

add_executable(kitti_player src/kitti_player.cpp)
target_link_libraries(kitti_player kitti)

add_executable(kitti_static_transform_broadcaster src/kitti_static_transform_broadcaster.cpp)
target_link_libraries(kitti_static_transform_broadcaster ${catkin_LIBRARIES})
