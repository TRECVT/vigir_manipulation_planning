cmake_minimum_required(VERSION 2.8.3)
project(vigir_ocs_robot_model)

find_package(Boost REQUIRED COMPONENTS system)

find_package(Boost REQUIRED system filesystem date_time program_options signals thread)
find_package(catkin REQUIRED COMPONENTS cmake_modules flor_utilities geometry_msgs flor_planning_msgs vigir_planning_msgs flor_ocs_msgs roscpp moveit_core moveit_ros_planning  flor_visualization_utils eigen_conversions vigir_moveit_utils)
#flor_moveit_tools
find_package(Eigen REQUIRED)

catkin_package(
   INCLUDE_DIRS
     include
   LIBRARIES
     ${PROJECT_NAME}
   CATKIN_DEPENDS
     flor_utilities
     geometry_msgs
     flor_planning_msgs
     vigir_planning_msgs
     vigir_moveit_utils
     moveit_core
     moveit_ros_planning
     #flor_moveit_tools
     roscpp
     eigen_conversions
#  DEPENDS system_lib
)

include_directories(include
                    ${catkin_INCLUDE_DIRS})

link_directories(${Boost_LIBRARY_DIRS})
link_directories(${catkin_LIBRARY_DIRS})

SET(HEADERS
include/${PROJECT_NAME}/moveit_ocs_model.h
include/${PROJECT_NAME}/moveit_ocs_model_ros.h
)


SET(SOURCE
src/moveit_ocs_model_node.cpp
src/moveit_ocs_model.cpp
src/moveit_ocs_model_ros.cpp
)

add_executable(moveit_ocs_model_node ${SOURCE} ${HEADERS} )
target_link_libraries(moveit_ocs_model_node ${catkin_LIBRARIES}  ${Boost_LIBRARIES})
add_dependencies(moveit_ocs_model_node ${catkin_EXPORTED_TARGETS})

add_library(${PROJECT_NAME} ${HEADERS} ${SOURCE})
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES} ${Boost_LIBRARIES})
add_dependencies(${PROJECT_NAME} ${catkin_EXPORTED_TARGETS})

#install targets
install( TARGETS ${PROJECT_NAME} moveit_ocs_model_node 
   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)



