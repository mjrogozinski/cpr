macro(set_cache_variable VAR_NAME VAR_DESCRIPTION)
    set(${VAR_NAME} ${${VAR_NAME}} CACHE INTERNAL ${VAR_DESCRIPTION})
    message(STATUS "Set ${VAR_NAME} to ${${VAR_NAME}}.")
endmacro()

# Code coverage

if(GENERATE_COVERAGE)
    set(CMAKE_BUILD_TYPE COVERAGE CACHE INTERNAL "Coverage enabled build")
    message(STATUS "Enabling gcov support")
    if(NOT "${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
        set(COVERAGE_FLAG "--coverage")
    endif()
    set(CMAKE_CXX_FLAGS_COVERAGE
        "-g -O0 ${COVERAGE_FLAG} -fprofile-arcs -ftest-coverage"
        CACHE STRING "Flags used by the C++ compiler during coverage builds."
        FORCE)
    set(CMAKE_C_FLAGS_COVERAGE
        "-g -O0 ${COVERAGE_FLAG} -fprofile-arcs -ftest-coverage"
        CACHE STRING "Flags used by the C compiler during coverage builds."
        FORCE)
    set(CMAKE_EXE_LINKER_FLAGS_COVERAGE
        ""
        CACHE STRING "Flags used for linking binaries during coverage builds."
        FORCE)
    set(CMAKE_SHARED_LINKER_FLAGS_COVERAGE
        ""
        CACHE STRING "Flags used by the shared libraries linker during coverage builds."
        FORCE)
    mark_as_advanced(
        CMAKE_CXX_FLAGS_COVERAGE
        CMAKE_C_FLAGS_COVERAGE
        CMAKE_EXE_LINKER_FLAGS_COVERAGE
        CMAKE_SHARED_LINKER_FLAGS_COVERAGE)
endif()

message(STATUS "Building mongoose project for test support.")
add_subdirectory(mongoose)

add_library(libmongoose-imported INTERFACE)
target_link_libraries(libmongoose-imported INTERFACE mongoose)
target_include_directories(libmongoose-imported INTERFACE ${mongoose_SOURCE_DIR})
add_library(mongoose::lib ALIAS libmongoose-imported)

