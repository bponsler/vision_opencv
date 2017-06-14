cmake_minimum_required(VERSION 2.8)
project(image_geometry)

# Add support for C++11
if(NOT WIN32)
  add_definitions(-std=c++11)
endif()

find_package(ament_cmake REQUIRED)
find_package(ament_cmake_python REQUIRED)
find_package(ros2_time REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(sensor_msgs_util REQUIRED)

find_package(OpenCV REQUIRED)

ament_python_install_package(${PROJECT_NAME})

set(INCLUDE_DIRS include ${ament_cmake_INCLUDE_DIRS} ${OpenCV_INCLUDE_DIRS}
  ${ros2_time_INCLUDE_DIRS} ${sensor_msgs_INCLUDE_DIRS} ${sensor_msgs_util_INCLUDE_DIRS})
include_directories(${INCLUDE_DIRS})

set(LIBRARY_DIRS ${ament_cmake_LIBRARY_DIRS} ${OpenCV_LIBRARY_DIRS}
  ${ros2_time_LIBRARY_DIRS} ${sensor_msgs_LIBRARY_DIRS} ${sensor_msgs_util_LIBRARY_DIRS})

link_directories(${LIBRARY_DIRS})

set(LIBS ${ament_cmake_LIBRARIES} ${OpenCV_LIBRARIES}
  ${ros2_time_LIBRARIES} ${sensor_msgs_LIBRARIES} ${sensor_msgs_util_LIBRARIES})

# add a library
add_library(${PROJECT_NAME} src/pinhole_camera_model.cpp
  src/stereo_camera_model.cpp)
target_link_libraries(${PROJECT_NAME} ${LIBS})

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION include/${PROJECT_NAME}/)

install(TARGETS ${PROJECT_NAME}
  RUNTIME
  DESTINATION lib/${PROJECT_NAME}
  ARCHIVE
  DESTINATION lib
  LIBRARY
  DESTINATION lib)

# add tests
if(BUILD_TESTING)
  add_subdirectory(test)
endif()

ament_export_dependencies(ament_cmake)
ament_export_dependencies(ament_cmake_python)
ament_export_dependencies(ros2_time)
ament_export_dependencies(sensor_msgs)
ament_export_dependencies(sensor_msgs_util)
ament_export_include_directories(${INCLUDE_DIRS})
ament_export_libraries(${PROJECT_NAME} ${LIBS})

ament_package()
