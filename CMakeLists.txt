cmake_minimum_required(VERSION 3.7.2)

project(rtl-sdr-unix
    VERSION 1.0
    LANGUAGES CXX C
    DESCRIPTION "dffdfds"
)
include(CMakePackageConfigHelpers)
# configure_file (
#     "${PROJECT_SOURCE_DIR}/cmake/version.h.in"
#     "${PROJECT_SOURCE_DIR}/include/involi/log/version.h"
#     @ONLY
# )
# configure_package_config_file(cmake/config.cmake.in
# 	${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}-config.cmake
# 	INSTALL_DESTINATION ${CMAKE_INSTALL_DATADIR}/${PROJECT_NAME}
# 	NO_SET_AND_CHECK_MACRO)
file(GLOB_RECURSE HEADER_LIST "include/*.h")

add_library(${PROJECT_NAME} INTERFACE)
add_library(rtl-sdr-unix::rtlsdr_static ALIAS ${PROJECT_NAME})

target_include_directories(${PROJECT_NAME} INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>  
    $<INSTALL_INTERFACE:include>
)


set(INSTALL_CMAKE_DIR "share/${PROJECT_NAME}")
set(PROJECT_CONFIG_FILE "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}-config.cmake")

install(
    TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}-targets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
)

install(
    DIRECTORY include/
    DESTINATION include
    FILES_MATCHING
        PATTERN "*.h"
)

configure_package_config_file(
    "cmake/${PROJECT_NAME}-config.cmake.in"
    "${PROJECT_CONFIG_FILE}"
    INSTALL_DESTINATION "${INSTALL_CMAKE_DIR}"
)

install(
    EXPORT ${PROJECT_NAME}-targets
    FILE ${PROJECT_NAME}-targets.cmake
    NAMESPACE involi::
    DESTINATION "${INSTALL_CMAKE_DIR}"
)

install(
    FILES "${PROJECT_CONFIG_FILE}"
    DESTINATION "${INSTALL_CMAKE_DIR}"
)



# set(rtl_VERSION 1.0.0)

# project(test VERSION rtl_VERSION DESCRIPTION "test description")
# include(GNUInstallDirs)
# include(CMakePackageConfigHelpers)
# add_subdirectory(src)
# target_include_directories(rtlsdr_static
# 	PUBLIC
# 		$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
# 		$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)
# # add_executable(test rtl_test.c convenience/convenience.c)
# # target_link_libraries(test PUBLIC rtlsdr_static m)

# # target_include_directories(test PUBLIC
# #                           "${PROJECT_BINARY_DIR}"
# #                           "${PROJECT_SOURCE_DIR}/src"
# #                           "${PROJECT_SOURCE_DIR}/convenience"
# #                           )

# # install(
# #     FILES
# #         ${CMAKE_CURRENT_BINARY_DIR}/beison-config.cmake
# #         ${CMAKE_CURRENT_BINARY_DIR}/beison-config-version.cmake
# #     DESTINATION
# #         ${CMAKE_INSTALL_DATADIR}/beison)
# configure_package_config_file(cmake/config.cmake.in
# 	${CMAKE_CURRENT_BINARY_DIR}/rtl-sdr-unix-config.cmake
# 	INSTALL_DESTINATION ${CMAKE_INSTALL_DATADIR}/rtl-sdr-unix
# 	NO_SET_AND_CHECK_MACRO)
# write_basic_package_version_file(
# 	${CMAKE_CURRENT_BINARY_DIR}/rtl-sdr-unix-config-version.cmake
# 	VERSION ${rtl_VERSION}
# 	COMPATIBILITY SameMajorVersion)
# install(
# 	FILES
# 		${CMAKE_CURRENT_BINARY_DIR}/rtl-sdr-unix-config.cmake
# 		${CMAKE_CURRENT_BINARY_DIR}/rtl-sdr-unix-config-version.cmake
# 	DESTINATION
# 		${CMAKE_INSTALL_DATADIR}/rtl-sdr-unix)
# install(TARGETS rtlsdr_static EXPORT rtlsdr-targets)
# install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
# install(EXPORT rtlsdr-targets
# NAMESPACE rtlsdr::
# DESTINATION ${CMAKE_INSTALL_DATADIR}/rtl-sdr-unix)