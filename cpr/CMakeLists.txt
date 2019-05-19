find_library(curl REQUIRED)

add_library(cpr
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
    )

target_link_libraries(cpr curl)

target_include_directories(cpr
    PUBLIC
    $<BUILD_INTERFACE:${cpr_SOURCE_DIR}/include/>
    $<INSTALL_INTERFACE:include>
    )

install(DIRECTORY ${cpr_SOURCE_DIR}/include/
    DESTINATION include
    )

include(GNUInstallDirs)

install(TARGETS cpr
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
