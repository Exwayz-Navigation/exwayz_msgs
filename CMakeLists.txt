cmake_minimum_required(VERSION 3.8)

project(exwayz_msgs)

# Find ROS and set ROS version-related variables
#------------------------------------------------------------------------------#
if(NOT DEFINED ENV{ROS_VERSION})
    message(FATAL_ERROR "Can't find ROS_VERSION environment variable, check ROS installation")
endif()

set(ROS_VERSION $ENV{ROS_VERSION})

message(STATUS "Found ROS version ${ROS_VERSION}")

# Add dependencies
#------------------------------------------------------------------------------#
if(${ROS_VERSION} EQUAL 1)
    find_package(catkin REQUIRED COMPONENTS roscpp rospy message_generation
            geometry_msgs std_msgs)

elseif(${ROS_VERSION} EQUAL 2)
    find_package(ament_cmake REQUIRED)
    find_package(builtin_interfaces REQUIRED)
    find_package(rosidl_typesupport_c REQUIRED)
    find_package(rosidl_default_generators REQUIRED)

    find_package(geometry_msgs REQUIRED)
    find_package(std_msgs REQUIRED)

endif()

# Build messages
#------------------------------------------------------------------------------#
if(${ROS_VERSION} EQUAL 1)
    file(GLOB msg_files RELATIVE ${CMAKE_CURRENT_LIST_DIR}/msg "msg/*.msg")

    add_message_files(DIRECTORY msg FILES ${msg_files})
    generate_messages(DEPENDENCIES std_msgs geometry_msgs)
    catkin_package(CATKIN_DEPENDS message_runtime roscpp rospy std_msgs)

elseif(${ROS_VERSION} EQUAL 2)
    file(GLOB msg_files RELATIVE ${CMAKE_CURRENT_LIST_DIR} "msg/*.msg")

    rosidl_generate_interfaces(exwayz_msgs ${msg_files}
            DEPENDENCIES builtin_interfaces geometry_msgs std_msgs)

    ament_export_dependencies(rosidl_default_runtime)
    ament_package()

endif()
