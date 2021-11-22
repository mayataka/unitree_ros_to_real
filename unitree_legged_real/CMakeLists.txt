cmake_minimum_required(VERSION 2.8.3)
project(unitree_legged_real CXX)
set(CMAKE_CXX_STANDARD 11)

find_package(catkin REQUIRED COMPONENTS
    roscpp
    geometry_msgs
    unitree_legged_msgs
)

find_package(lcm REQUIRED)

catkin_package()

include_directories(
    include
    ${Boost_INCLUDE_DIR}
    ${catkin_INCLUDE_DIRS}
)

set(CMAKE_CXX_FLAGS "-O3")


macro(add_legged_sdk_dependencies EXECUTABLE)
    target_link_libraries(
        ${EXECUTABLE}
        ${UNITREE_LEGGED_SDK_DIR}/lib/libunitree_legged_sdk_amd64.so 
        ${catkin_LIBRARIES}
        lcm::lcm
    )
    add_dependencies(
        ${EXECUTABLE}
         ${${PROJECT_NAME}_EXPORTED_TARGETS} 
         ${catkin_EXPORTED_TARGETS}
    )
    target_include_directories(
        ${EXECUTABLE}
        PUBLIC 
        ${UNITREE_LEGGED_SDK_DIR}/include
        ${LCM_INCLUDE_DIR}/include
    )
endmacro()


add_executable(lcm_server src/exe/lcm_server.cpp)
add_legged_sdk_dependencies(lcm_server)

add_executable(position_lcm src/exe/position_mode.cpp)
add_legged_sdk_dependencies(position_lcm)

add_executable(velocity_lcm src/exe/velocity_mode.cpp)
add_legged_sdk_dependencies(velocity_lcm)

add_executable(torque_lcm src/exe/torque_mode.cpp)
add_legged_sdk_dependencies(torque_lcm)

add_executable(walk_lcm src/exe/walk_mode.cpp)
add_legged_sdk_dependencies(walk_lcm)