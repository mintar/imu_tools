cmake_minimum_required(VERSION 2.8.3)
project(imu_filter_madgwick)

find_package(catkin REQUIRED COMPONENTS roscpp sensor_msgs geometry_msgs tf nodelet message_filters dynamic_reconfigure)

find_package(Boost REQUIRED COMPONENTS system thread signals)

# Generate dynamic parameters
generate_dynamic_reconfigure_options(cfg/ImuFilterMadgwick.cfg)


catkin_package(
  DEPENDS 
  CATKIN_DEPENDS roscpp sensor_msgs geometry_msgs tf nodelet message_filters dynamic_reconfigure
  INCLUDE_DIRS
  LIBRARIES imu_filter imu_filter_nodelet
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
)

link_directories(${catkin_LIBRARY_DIRS})


# create imu_filter library
add_library (imu_filter src/imu_filter.cpp)
target_link_libraries(imu_filter ${catkin_LIBRARIES} ${Boost_LIBRARIES})

# create imu_filter_nodelet library
add_library (imu_filter_nodelet src/imu_filter_nodelet.cpp)
target_link_libraries(imu_filter_nodelet imu_filter ${catkin_LIBRARIES} ${Boost_LIBRARIES})

# create imu_filter_node executable
add_executable(imu_filter_node src/imu_filter_node.cpp)
target_link_libraries(imu_filter_node imu_filter ${catkin_LIBRARIES} ${Boost_LIBRARIES})


install(TARGETS imu_filter imu_filter_nodelet imu_filter_node
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)