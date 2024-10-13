cmake_minimum_required(VERSION 3.5)
project(plan_env)

set(CMAKE_BUILD_TYPE "Release")
ADD_COMPILE_OPTIONS(-std=c++17 )
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -Wall -g")

find_package(OpenCV REQUIRED)


find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(nav_msgs REQUIRED)
find_package(pcl_conversions REQUIRED)
find_package(visualization_msgs REQUIRED)
find_package(cv_bridge REQUIRED)
find_package(message_filters REQUIRED)

find_package(Eigen3 REQUIRED)
find_package(PCL 1.7 REQUIRED)

include_directories( include 
    ${Eigen3_INCLUDE_DIRS} 
    ${PCL_INCLUDE_DIRS}
    ${OpenCV_INCLUDE_DIRS}
)

link_directories(${PCL_LIB_DIR})

add_library( plan_env SHARED
    src/grid_map.cpp 
    src/raycast.cpp
    src/obj_predictor.cpp 
    )

ament_target_dependencies(plan_env 
    cv_bridge
    nav_msgs
    pcl_conversions
    rclcpp
    sensor_msgs
    visualization_msgs
  )

target_link_libraries( plan_env
    ${PCL_LIBRARIES}
    ${OpenCV_LIBS}
    )  

add_executable(obj_generator
    src/obj_generator.cpp 
)
target_link_libraries(obj_generator 
    plan_env
    )

install(TARGETS
    plan_env
    obj_generator
    DESTINATION lib/${PROJECT_NAME}
    RUNTIME DESTINATION lib/${PROJECT_NAME}
)

ament_package()
