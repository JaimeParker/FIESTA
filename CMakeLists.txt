cmake_minimum_required(VERSION 2.8.3)
project(fiesta)

set(CMAKE_CXX_FLAGS "-std=c++17 ${CMAKE_CXX_FLAGS} -O3 -Wall -pthread") # -Wextra -Werror
set(CMAKE_BUILD_TYPE "RELEASE")

find_package(catkin REQUIRED COMPONENTS
        roscpp
        std_msgs
        nav_msgs
        visualization_msgs
        tf
        cv_bridge
        )

find_package(Eigen3 REQUIRED)
find_package(PCL 1.7 REQUIRED)
find_package(OpenCV REQUIRED)
set(Eigen3_INCLUDE_DIRS ${EIGEN3_INCLUDE_DIR})

catkin_package(
        INCLUDE_DIRS include
        LIBRARIES fiesta_lib
        CATKIN_DEPENDS roscpp std_msgs nav_msgs visualization_msgs tf cv_bridge
        DEPENDS EIGEN3 PCL OpenCV
)


include_directories(${PROJECT_SOURCE_DIR}/include)

include_directories(
    include
    SYSTEM
    ${catkin_INCLUDE_DIRS}
    ${Eigen3_INCLUDE_DIRS}
    ${PCL_INCLUDE_DIRS}
    ${OpenCV_INCLUDE_DIRS}
)

link_directories(${PCL_LIBRARY_DIRS})

add_library(fiesta_lib
        src/ESDFMap.cpp
        src/raycast.cpp
        src/parameters.cpp
        src/timing.cc
)

target_include_directories(fiesta_lib PUBLIC
        ${catkin_INCLUDE_DIRS}
        ${PCL_INCLUDE_DIRS}
        ${OpenCV_INCLUDE_DIRS}
        ${Eigen3_INCLUDE_DIRS}
)

target_link_libraries(fiesta_lib
        ${catkin_LIBRARIES}
        ${PCL_LIBRARIES}
        ${OpenCV_LIBS}
)

add_executable(test_fiesta 
        test/test_fiesta.cpp
)
target_link_libraries(test_fiesta
        fiesta_lib
)

