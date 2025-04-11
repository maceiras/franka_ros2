cmake_minimum_required(VERSION 3.5)
project(franka_semantic_components)

# Default to C++17
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

option(CHECK_TIDY "Adds clang-tidy tests" OFF)

set(THIS_PACKAGE_INCLUDE_DEPENDS Franka franka_hardware franka_msgs hardware_interface controller_interface rclcpp_lifecycle)

# find dependencies
find_package(ament_cmake REQUIRED)
foreach(Dependency IN ITEMS ${THIS_PACKAGE_INCLUDE_DEPENDS})
  find_package(${Dependency} REQUIRED)
endforeach()

add_library(franka_semantic_components SHARED src/franka_robot_state.cpp src/franka_robot_model.cpp)
target_include_directories(franka_semantic_components PRIVATE include)
target_link_libraries(franka_semantic_components Franka::Franka)
ament_target_dependencies(franka_semantic_components ${THIS_PACKAGE_INCLUDE_DEPENDS})

install(TARGETS franka_semantic_components DESTINATION lib)
install(DIRECTORY include/ DESTINATION include)

ament_export_include_directories(include)
ament_export_libraries(franka_semantic_components)
ament_export_dependencies(${THIS_PACKAGE_INCLUDE_DEPENDS})
ament_package()
