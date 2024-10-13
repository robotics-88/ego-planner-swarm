cmake_minimum_required(VERSION 3.5)
project(traj_utils)

set(CMAKE_BUILD_TYPE "Release")
ADD_COMPILE_OPTIONS(-std=c++17 )
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -Wall -g")

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(visualization_msgs REQUIRED)
find_package(rosidl_default_generators REQUIRED)

find_package(Eigen3 REQUIRED)
find_package(PCL 1.7 REQUIRED)

include_directories( include 
    ${Eigen3_INCLUDE_DIRS} 
    ${PCL_INCLUDE_DIRS}
)

add_library( traj_utils 
    src/planning_visualization.cpp 
    src/polynomial_traj.cpp 
    )

ament_target_dependencies(traj_utils 
    std_msgs
    geometry_msgs
    rclcpp
    visualization_msgs
  )
# Generate messages in the 'msg' folder
set(msg_files
  msg/Bspline.msg
  msg/DataDisp.msg
  msg/MultiBsplines.msg
)

# Generate added messages and services with any dependencies listed here
rosidl_generate_interfaces(${PROJECT_NAME}_msgs
  ${msg_files}
  DEPENDENCIES
  geometry_msgs
  std_msgs
)

link_directories(${PCL_LIB_DIR})

install(TARGETS
  traj_utils
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib/${PROJECT_NAME}
)

ament_package()
