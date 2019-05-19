cmake_minimum_required(VERSION 3.12)

project(cpr CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

message(STATUS "CMake Options")
message(STATUS "=======================================================")
option(BUILD_CPR_TESTS "Set to ON to build cpr tests." OFF)
option(GENERATE_COVERAGE "Set to ON to generate coverage reports." OFF)
option(CPR_CURL_NOSIGNAL "Set to ON to disable use of signals in libcurl." OFF)
option(CMAKE_USE_OPENSSL "Use OpenSSL code. Experimental" ON)
message(STATUS "=======================================================")

add_subdirectory(cpr)

if(BUILD_CPR_TESTS)
    enable_testing()
    add_subdirectory(opt)
    add_subdirectory(test)
endif()
