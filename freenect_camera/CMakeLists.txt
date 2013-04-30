cmake_minimum_required(VERSION 2.8.3)
project(freenect_camera)
find_package(catkin REQUIRED
             COMPONENTS
             camera_info_manager
             diagnostic_updater
             dynamic_reconfigure
             image_transport
             nodelet
             roscpp
             sensor_msgs)

# Resolve system dependency on libfreenect, which does not provide a
# CMake find_package() module.
find_package(PkgConfig REQUIRED)
pkg_check_modules(LIBFREENECT REQUIRED libfreenect)
find_path(LIBFREENECT_INCLUDE_DIR
  NAMES libfreenect.h
  PATHS ${LIBFREENECT_INCLUDE_DIRS}
)
find_library(LIBFREENECT_LIBRARY
  NAMES freenect
  PATHS ${LIBFREENECT_LIBRARY_DIRS}
)

# auto-generate dynamic reconfiguration GUI before catkin_package()
generate_dynamic_reconfigure_options(cfg/Freenect.cfg)

include_directories(include ${catkin_INCLUDE_DIRS} ${LIBFREENECT_INCLUDE_DIR})

# CATKIN_MIGRATION: removed during catkin migration
# rosbuild_add_boost_directories()

#find_package(Boost REQUIRED COMPONENTS thread)
#target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES})
#add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})

add_executable(freenect_node src/nodes/freenect_node.cpp)
target_link_libraries(freenect_node ${catkin_LIBRARIES} ${LIBFREENECT_LIBRARIES})

add_library(freenect_nodelet src/nodelets/driver.cpp)

catkin_package(
    DEPENDS
    libfreenect
    CATKIN_DEPENDS
    camera_info_manager
    diagnostic_updater
    dynamic_reconfigure
    image_transport
    nodelet
    roscpp
    sensor_msgs
)
