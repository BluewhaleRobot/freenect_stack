cmake_minimum_required(VERSION 2.8.3)
project(freenect_launch)

find_package(catkin REQUIRED
             COMPONENTS
             depth_image_proc
             freenect_camera
             image_proc
             nodelet
             tf)

catkin_package()

# install the launch files
install(DIRECTORY launch/
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch)
