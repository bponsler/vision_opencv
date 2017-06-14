# add the tests

# add boost directories for now
include_directories("../src")

ament_add_gtest(${PROJECT_NAME}-utest test_endian.cpp test_compression.cpp utest.cpp utest2.cpp test_rgb_colors.cpp)
target_link_libraries(${PROJECT_NAME}-utest
  ${PROJECT_NAME}
  ${OpenCV_LIBRARIES}
  ${LIBS}
)

ament_add_nose_test(${PROJECT_NAME}_enum_nost_test enumerants.py)
ament_add_nose_test(${PROJECT_NAME}_conversion_nose_test conversions.py)
ament_add_nose_test(${PROJECT_NAME}_py_binding_nose_test python_bindings.py)
