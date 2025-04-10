add_subdirectory(wpt)

set(ADA_TEST_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/../)
if(MSVC)
  add_compile_options("/Zi" "/EHsc" "/GR")
endif()

include(${PROJECT_SOURCE_DIR}/cmake/add-cpp-test.cmake)
link_libraries(ada)

include(GoogleTest)

add_executable(wpt_tests wpt_tests.cpp)
add_executable(url_components url_components.cpp)
add_executable(basic_tests basic_tests.cpp)
add_executable(from_file_tests from_file_tests.cpp)
add_executable(ada_c ada_c.cpp)

target_link_libraries(wpt_tests PRIVATE simdjson GTest::gtest_main)
target_link_libraries(url_components PRIVATE simdjson GTest::gtest_main)
target_link_libraries(basic_tests PRIVATE simdjson GTest::gtest_main)
target_link_libraries(from_file_tests PRIVATE simdjson GTest::gtest_main)
target_link_libraries(ada_c PRIVATE simdjson GTest::gtest_main)

gtest_discover_tests(wpt_tests)
gtest_discover_tests(url_components)
gtest_discover_tests(basic_tests)
gtest_discover_tests(from_file_tests)
gtest_discover_tests(ada_c)

add_cpp_test(basic_fuzzer)

if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
  if (CMAKE_CXX_COMPILER_VERSION VERSION_LESS 9)
    target_link_libraries(wpt_tests PUBLIC stdc++fs)
    target_link_libraries(url_components PUBLIC stdc++fs)
  endif()
endif()

if(MSVC OR MINGW)
  target_compile_definitions(wpt_tests PRIVATE _CRT_SECURE_NO_WARNINGS)
  target_compile_definitions(url_components PRIVATE _CRT_SECURE_NO_WARNINGS)
  target_compile_definitions(basic_fuzzer PRIVATE _CRT_SECURE_NO_WARNINGS)
  target_compile_definitions(from_file_tests PRIVATE _CRT_SECURE_NO_WARNINGS)
  target_compile_definitions(basic_tests PRIVATE _CRT_SECURE_NO_WARNINGS)
endif()
