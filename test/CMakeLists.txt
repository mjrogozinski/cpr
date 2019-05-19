find_package(Threads)
find_package(GTest REQUIRED)

add_library(test_server
    server.cpp
    )

target_link_libraries(test_server
    mongoose::lib
    ${CMAKE_THREAD_LIBS_INIT}
    )

macro(add_cpr_test _TEST_NAME)
    add_executable(${_TEST_NAME}_tests
        ${_TEST_NAME}_tests.cpp)
    target_link_libraries(${_TEST_NAME}_tests
        test_server
        GTest::GTest
        GTest::Main
        cpr)
    add_test(NAME cpr_${_TEST_NAME}_tests COMMAND ${_TEST_NAME}_tests)
    # Group under the "tests" project folder in IDEs such as Visual Studio.
    set_property(TARGET ${_TEST_NAME}_tests PROPERTY FOLDER "tests")
endmacro()

add_cpr_test(get)
add_cpr_test(post)
add_cpr_test(session)
add_cpr_test(async)
add_cpr_test(proxy)
add_cpr_test(head)
add_cpr_test(delete)
add_cpr_test(put)
add_cpr_test(callback)
add_cpr_test(raw_body)
add_cpr_test(options)
add_cpr_test(patch)
add_cpr_test(error)
add_cpr_test(alternating)
add_cpr_test(util)
