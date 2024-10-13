cmake_minimum_required(VERSION 3.5)
project(traj_utils)

set(CMAKE_BUILD_TYPE "Release")
ADD_COMPILE_OPTIONS(-std=c++17 )
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -Wall -g")

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(rosidl_default_generators REQUIRED)

find_package(Eigen3 REQUIRED)
find_package(PCL 1.7 REQUIRED)

# Generate messages in the 'msg' folder
set(msg_files
  msg/Bspline.msg
  msg/DataDisp.msg
  msg/MultiBsplines.msg
)

# Generate added messages and services with any dependencies listed here
rosidl_generate_interfaces(${PROJECT_NAME}
  ${msg_files}
  DEPENDENCIES
  geometry_msgs
  std_msgs
)

include_directories( include 
    ${Eigen3_INCLUDE_DIRS} 
    ${PCL_INCLUDE_DIRS}
)

link_directories(${PCL_LIB_DIR})

add_library( traj_utils SHARED
    src/planning_visualization.cpp 
    src/polynomial_traj.cpp 
    )

ament_package()
