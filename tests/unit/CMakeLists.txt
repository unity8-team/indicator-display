set(SERVICE_LINK_LIBRARIES
    ${SERVICE_LIB}
    ${SERVICE_DEPS_LIBRARIES}
)
set(QT_LINK_LIBRARIES
    test-utils
    Qt5::Core
    Qt5::Test
    Qt5::DBus
)
set(TEST_LINK_LIBRARIES
    ${TEST_DEPS_LIBRARIES}
    ${GTEST_LIBRARIES}
    ${GMOCK_LIBRARIES}
)

add_definitions(
  -DGREETER_TEMPLATE="${CMAKE_SOURCE_DIR}/tests/utils/mock-unity-greeter.py"
)

function(add_test_by_name name)
  set(TEST_NAME ${name})
  set(COVERAGE_TEST_TARGETS ${COVERAGE_TEST_TARGETS} ${TEST_NAME} PARENT_SCOPE)
  add_executable (${TEST_NAME} ${TEST_NAME}.cpp)
  add_test(${TEST_NAME} ${TEST_NAME})
  set_property(TEST ${TEST_NAME} APPEND PROPERTY ENVIRONMENT ${CTEST_ENVIRONMENT})
  target_link_libraries(${TEST_NAME} ${SERVICE_LINK_LIBRARIES} ${TEST_LINK_LIBRARIES} ${THREAD_LINK_LIBRARIES})
endfunction()
add_test_by_name(adbd-client-test)
add_test_by_name(rotation-lock-test)

function(add_qt_test_by_name name)
  set(TEST_NAME ${name})
  set(COVERAGE_TEST_TARGETS ${COVERAGE_TEST_TARGETS} ${TEST_NAME} PARENT_SCOPE)
  add_executable (${TEST_NAME} ${TEST_NAME}.cpp)
  add_test(${TEST_NAME} ${TEST_NAME})
  set_property(TEST ${TEST_NAME} APPEND PROPERTY ENVIRONMENT ${CTEST_ENVIRONMENT})
  target_link_libraries(${TEST_NAME} ${SERVICE_LINK_LIBRARIES} ${QT_LINK_LIBRARIES} ${TEST_LINK_LIBRARIES} ${THREAD_LINK_LIBRARIES})
endfunction()
add_qt_test_by_name(greeter-test)
add_qt_test_by_name(usb-snap-test)

set(COVERAGE_TEST_TARGETS
  ${COVERAGE_TEST_TARGETS}
  PARENT_SCOPE
)
