cmake_minimum_required(VERSION 3.7.2)

set(rtl-sdr-unix_VERSION 1.0.0)

project(test VERSION rtl-sdr-unix_VERSION DESCRIPTION "test description")
include(GNUInstallDirs)
include(CMakePackageConfigHelpers)
add_subdirectory(src)
target_include_directories(rtlsdr_static
	PUBLIC
		$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
		$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>)
# add_executable(test rtl_test.c convenience/convenience.c)
# target_link_libraries(test PUBLIC rtlsdr_static m)

# target_include_directories(test PUBLIC
#                           "${PROJECT_BINARY_DIR}"
#                           "${PROJECT_SOURCE_DIR}/src"
#                           "${PROJECT_SOURCE_DIR}/convenience"
#                           )

# install(
#     FILES
#         ${CMAKE_CURRENT_BINARY_DIR}/beison-config.cmake
#         ${CMAKE_CURRENT_BINARY_DIR}/beison-config-version.cmake
#     DESTINATION
#         ${CMAKE_INSTALL_DATADIR}/beison)
configure_package_config_file(cmake/config.cmake.in
	${CMAKE_CURRENT_BINARY_DIR}/rtl-sdr-unix-config.cmake
	INSTALL_DESTINATION ${CMAKE_INSTALL_DATADIR}/rtl-sdr-unix
	NO_SET_AND_CHECK_MACRO)
write_basic_package_version_file(
	${CMAKE_CURRENT_BINARY_DIR}/rtl-sdr-unix-config-version.cmake
	VERSION ${rtl-sdr-unix_VERSION}
	COMPATIBILITY SameMajorVersion)
install(
	FILES
		${CMAKE_CURRENT_BINARY_DIR}/rtl-sdr-unix-config.cmake
		${CMAKE_CURRENT_BINARY_DIR}/rtl-sdr-unix-config-version.cmake
	DESTINATION
		${CMAKE_INSTALL_DATADIR}/rtl-sdr-unix)
install(TARGETS rtlsdr_static EXPORT rtlsdr-targets)
install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
install(EXPORT rtlsdr-targets
NAMESPACE rtlsdr::
DESTINATION ${CMAKE_INSTALL_DATADIR}/rtl-sdr-unix)