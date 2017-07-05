cmake_minimum_required(VERSION 3.5)
project(cv_bridge)

# Add support for C++11
if(NOT WIN32)
  add_definitions(-std=c++11)
endif()

find_package(ament_cmake REQUIRED)
find_package(ament_cmake_python REQUIRED)
find_package(ros2_console REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(sensor_msgs_util REQUIRED)

ament_python_install_package(${PROJECT_NAME})

set(INCLUDE_DIRS include ${ament_cmake_INCLUDE_DIRS} ${ros2_console_INCLUDE_DIRS}
  ${sensor_msgs_INCLUDE_DIRS} ${sensor_msgs_util_INCLUDE_DIRS})
include_directories(${INCLUDE_DIRS})

set(LIBRARY_DIRS ${ament_cmake_LIBRARY_DIRS} ${ros2_console_LIBRARY_DIRS}
  ${sensor_msgs_LIBRARY_DIRS} ${sensor_msgs_util_LIBRARY_DIRS})

link_directories(${LIBRARY_DIRS})

set(LIBS ${ament_cmake_LIBRARIES} ${ros2_console_LIBRARIES}
  ${sensor_msgs_LIBRARIES} ${sensor_msgs_util_LIBRARIES})

if(NOT ANDROID)
  find_package(PythonLibs)
  if(PYTHONLIBS_VERSION_STRING VERSION_LESS 3)
    find_package(Boost REQUIRED python)
  else()
    find_package(Boost REQUIRED python-py35)
  endif()
else()
  find_package(Boost REQUIRED)
endif()
find_package(OpenCV REQUIRED)

if(NOT ANDROID)
  add_subdirectory(python)
endif()
add_subdirectory(src)
if(BUILD_TESTING)
  add_subdirectory(test)
endif()

# install the include folder
install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION include/${PROJECT_NAME})

ament_export_dependencies(ament_cmake)
ament_export_dependencies(ros2_console)
ament_export_dependencies(sensor_msgs)
ament_export_dependencies(sensor_msgs_util)
ament_export_include_directories(${INCLUDE_DIRS})

ament_package()
