message(STATUS "Using CURL_INCLUDE_DIRS: ${CURL_INCLUDE_DIRS}.")

add_library(${CPR_LIBRARIES}

    # Source files
    auth.cpp
    cookies.cpp
    cprtypes.cpp
    digest.cpp
    error.cpp
    multipart.cpp
    parameters.cpp
    payload.cpp
    proxies.cpp
    session.cpp
    timeout.cpp
    util.cpp
    ssl_options.cpp

    # Header files (useful in IDEs)
    "${CPR_INCLUDE_DIRS}/cpr/api.h"
    "${CPR_INCLUDE_DIRS}/cpr/auth.h"
    "${CPR_INCLUDE_DIRS}/cpr/body.h"
    "${CPR_INCLUDE_DIRS}/cpr/cookies.h"
    "${CPR_INCLUDE_DIRS}/cpr/cpr.h"
    "${CPR_INCLUDE_DIRS}/cpr/cprtypes.h"
    "${CPR_INCLUDE_DIRS}/cpr/curlholder.h"
    "${CPR_INCLUDE_DIRS}/cpr/defines.h"
    "${CPR_INCLUDE_DIRS}/cpr/digest.h"
    "${CPR_INCLUDE_DIRS}/cpr/error.h"
    "${CPR_INCLUDE_DIRS}/cpr/max_redirects.h"
    "${CPR_INCLUDE_DIRS}/cpr/multipart.h"
    "${CPR_INCLUDE_DIRS}/cpr/parameters.h"
    "${CPR_INCLUDE_DIRS}/cpr/payload.h"
    "${CPR_INCLUDE_DIRS}/cpr/proxies.h"
    "${CPR_INCLUDE_DIRS}/cpr/response.h"
    "${CPR_INCLUDE_DIRS}/cpr/session.h"
    "${CPR_INCLUDE_DIRS}/cpr/timeout.h"
    "${CPR_INCLUDE_DIRS}/cpr/util.h"
    "${CPR_INCLUDE_DIRS}/cpr/ssl_options.h")

message(STATUS "Using CURL_LIBRARIES: ${CURL_LIBRARIES}.")
target_link_libraries(${CPR_LIBRARIES}
    ${CURL_LIBRARIES})

if((CMAKE_VERSION VERSION_LESS 3.0))
    include_directories(
        ${CPR_INCLUDE_DIRS}
        ${CURL_INCLUDE_DIRS})
    return()
endif()

target_include_directories(${CPR_LIBRARIES}
    PRIVATE ${CURL_INCLUDE_DIRS}
    PUBLIC
    $<BUILD_INTERFACE:${CPR_INCLUDE_DIRS}>
    $<INSTALL_INTERFACE:include>)

install(DIRECTORY ${cpr_SOURCE_DIR}/include/
    DESTINATION include
    )

include(GNUInstallDirs)

install(TARGETS cpr libcurl
    EXPORT cpr-targets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    )

install(EXPORT cpr-targets
    NAMESPACE cpr::
    FILE cprConfig.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/cpr
    )
