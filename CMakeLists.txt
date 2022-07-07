cmake_minimum_required(VERSION 3.7.2)
project(test VERSION 1.0.1 DESCRIPTION "test description")
include(GNUInstallDirs)
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

install(TARGETS rtlsdr_static EXPORT rtlsdr-targets)
install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
install(EXPORT rtlsdr-targets
NAMESPACE rtlsdr::
DESTINATION ${CMAKE_INSTALL_DATADIR}/rtl-sdr-unix)