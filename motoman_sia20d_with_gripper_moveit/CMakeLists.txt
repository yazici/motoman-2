cmake_minimum_required(VERSION 2.8.3)
project(motoman_sia20d_with_gripper_moveit)

add_compile_options(-std=c++11)

find_package(catkin REQUIRED)

catkin_package()

install(DIRECTORY launch DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
  PATTERN "setup_assistant.launch" EXCLUDE)
install(DIRECTORY config DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

find_package(Eigen3 REQUIRED)

# Eigen 3.2 (Wily) only provides EIGEN3_INCLUDE_DIR, not EIGEN3_INCLUDE_DIRS
if(NOT EIGEN3_INCLUDE_DIRS)
  set(EIGEN3_INCLUDE_DIRS ${EIGEN3_INCLUDE_DIR})
endif()

find_package(catkin REQUIRED
  COMPONENTS
    moveit_core
    stomp_core
    moveit_experimental
    moveit_visual_tools
    moveit_ros_planning
    moveit_ros_planning_interface
    pluginlib
    geometric_shapes
)

find_package(Boost REQUIRED system filesystem date_time thread)


catkin_package(
  LIBRARIES
    interactivity_utils
  INCLUDE_DIRS
    ${THIS_PACKAGE_INCLUDE_DIRS}
  CATKIN_DEPENDS
    moveit_core
    stomp_core
    moveit_experimental
    moveit_visual_tools
    moveit_ros_planning_interface
    interactive_markers
  DEPENDS
    EIGEN3
)

include_directories(SYSTEM ${THIS_PACKAGE_INCLUDE_DIRS} ${Boost_INCLUDE_DIR} ${EIGEN3_INCLUDE_DIRS})

include_directories(include ${catkin_INCLUDE_DIRS} ${Boost_INCLUDE_DIR} ${EIGEN3_INCLUDE_DIRS})

include_directories(${catkin_INCLUDE_DIRS})
link_directories(${catkin_LIBRARY_DIRS})

# acknowledges the c++ code as a ROS node
add_executable(pick_place src/pick_place.cpp)
target_link_libraries(pick_place ${catkin_LIBRARIES} ${Boost_LIBRARIES})
install(TARGETS pick_place DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
