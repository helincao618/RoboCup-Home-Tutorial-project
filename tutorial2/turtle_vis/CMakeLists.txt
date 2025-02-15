cmake_minimum_required(VERSION 2.8.3)
project(turtle_vis)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages

SET(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

find_package(catkin REQUIRED COMPONENTS
  roscpp
  sensor_msgs
  std_msgs
  tf
  tf_conversions
  visualization_msgs
  message_generation
)

## System dependencies are found with CMake's conventions


##############################################################################
# Eigen
##############################################################################
find_package(Eigen3 REQUIRED)
add_definitions(${EIGEN_DEFINITIONS})

##############################################################################
# Messages, Services and Actions
##############################################################################
#add_message_files(
 #  DIRECTORY msg
  # FILES
   #>>>>TODO: ADD THE MSG FILES
   #DesiredPose.msg
#)

## Generate services 
add_service_files(
  DIRECTORY srv
  FILES
  #>>>>TODO: ADD THE SERVICE FILES
  send_desired_pose.srv
)


## Generate added messages and services with any dependencies listed here
generate_messages(
  DEPENDENCIES
  std_msgs
)


###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
  INCLUDE_DIRS include
  LIBRARIES turtle_vis#>>>>TODO: ADD THE NAME OF THE LIBRARY GENERATED BY THIS PACKAGE
  CATKIN_DEPENDS roscpp rospy std_msgs tf tf_conversions visualization_msgs message_run
# DEPENDS system_lib
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
# include_directories(include)
include_directories(
  ${catkin_INCLUDE_DIRS}
  include
  ${roscpp_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIR}
  ${CMAKE_CURRENT_BINARY_DIR} #Needed to find all headers generated by the uic (Qt)
)

# Define the library target >>> TODO: Uncomment the below lines to include your library
add_library(turtle_ros_tutorial src/solutions/myClass/TurtleClass.cpp)

target_link_libraries(turtle_ros_tutorial ${roscpp_LIBRARIES} ${catkin_LIBRARIES})

set_target_properties(turtle_ros_tutorial  PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)

add_dependencies(turtle_ros_tutorial ${catkin_EXPORTED_TARGETS} ${${PROJECT_NAME}_EXPORTED_TARGETS})

##############################################################################
# Sections Library Enable for Releases
##############################################################################
#link_directories(${PROJECT_SOURCE_DIR}/lib)

##############################################################################
# Sections Tutorials
##############################################################################

# VISUALIZATION NODE
#add_executable(turtle_vis_node src/solutions/turtle_vis_node.cpp)
#target_link_libraries(turtle_vis_node ${catkin_LIBRARIES} ${roscpp_LIBRARIES} turtle_ros_tutorial)

# CONTROL NODE
add_executable(turtle_control src/solutions/turtle_control_node.cpp)
target_link_libraries(turtle_control ${catkin_LIBRARIES} ${roscpp_LIBRARIES} turtle_ros_tutorial)
add_dependencies(turtle_control ${catkin_EXPORTED_TARGETS} ${${PROJECT_NAME}_EXPORTED_TARGETS})

# SET DESIRED POSITION NODE
add_executable(turtle_set_position src/solutions/turtle_set_position.cpp)
target_link_libraries(turtle_set_position ${catkin_LIBRARIES} ${roscpp_LIBRARIES})
add_dependencies(turtle_set_position ${catkin_EXPORTED_TARGETS} ${${PROJECT_NAME}_EXPORTED_TARGETS})








