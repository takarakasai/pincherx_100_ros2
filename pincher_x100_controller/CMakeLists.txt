﻿################################################################################
# Set minimum required version of cmake, project name and compile options
################################################################################
cmake_minimum_required(VERSION 3.5)
project(pincher_x100_controller)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

################################################################################
# Find and load build settings from external packages
################################################################################
find_package(ament_cmake REQUIRED)
find_package(cmake_modules REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(open_manipulator_msgs REQUIRED)
find_package(pincher_x100_libs REQUIRED)
find_package(rclcpp REQUIRED)
find_package(robotis_manipulator REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(std_msgs REQUIRED)
find_package(trajectory_msgs REQUIRED)

# If you want to use MoveIt!, switch OFF to ON.
set(USE_MOVEIT OFF)

if (USE_MOVEIT)
  #find_package(moveit_core REQUIRED)
  #find_package(moveit_msgs REQUIRED)
  #find_package(moveit_ros_planning REQUIRED) 
  #find_package(moveit_ros_planning_interface REQUIRED)
endif ()

################################################################################
# Build
################################################################################
include_directories(
  include
)

if(USE_MOVEIT)
  set(dependencies
    "cmake_modules"
    "geometry_msgs" 
    "open_manipulator_msgs"
    "pincher_x100_libs" 
    "rclcpp" 
    "robotis_manipulator" 
    "sensor_msgs" 
    "std_msgs" 
    "trajectory_msgs" 
    "moveit_core"
    "moveit_msgs"
    "moveit_ros_planning"
    "moveit_ros_planning_interface"
  )
else()
  set(dependencies
    "cmake_modules"
    "geometry_msgs" 
    "open_manipulator_msgs"
    "pincher_x100_libs" 
    "rclcpp" 
    "robotis_manipulator" 
    "sensor_msgs" 
    "std_msgs" 
    "trajectory_msgs" 
  )
endif ()

set(EXEC_NAME "pincher_x100_controller")

add_executable(${EXEC_NAME} src/pincher_x100_controller.cpp)
ament_target_dependencies(${EXEC_NAME} ${dependencies})

################################################################################
# Install
################################################################################
install(TARGETS ${EXEC_NAME}
  DESTINATION lib/${PROJECT_NAME}
)

install(FILES 99-pincher-cdc.rules
  DESTINATION share/${PROJECT_NAME}
)

install(DIRECTORY launch param
  DESTINATION share/${PROJECT_NAME}
)

install(PROGRAMS scripts/create_udev_rules
  DESTINATION lib/${PROJECT_NAME}
)

################################################################################
# Macro for ament package
################################################################################
ament_export_include_directories(include)
ament_export_dependencies(cmake_modules)
ament_export_dependencies(geometry_msgs)
ament_export_dependencies(open_manipulator_msgs)
ament_export_dependencies(pincher_x100_libs)
ament_export_dependencies(rclcpp)
ament_export_dependencies(robotis_manipulator)
ament_export_dependencies(sensor_msgs)
ament_export_dependencies(std_msgs)
ament_export_dependencies(trajectory_msgs)
if (USE_MOVEIT)
  #ament_export_dependencies(moveit_core)
  #ament_export_dependencies(moveit_msgs)
  #ament_export_dependencies(moveit_ros_planning)
  #ament_export_dependencies(moveit_ros_planning_interface)
endif ()
ament_package()
