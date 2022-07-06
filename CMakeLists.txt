cmake_minimum_required(VERSION 3.7.2)
project(test VERSION 1.0.1 DESCRIPTION "test description")

add_subdirectory(src)

add_executable(test rtl_test.c convenience/convenience.c)
target_link_libraries(test PUBLIC rtlsdr_static m)

target_include_directories(test PUBLIC
                          "${PROJECT_BINARY_DIR}"
                          "${PROJECT_SOURCE_DIR}/src"
                          "${PROJECT_SOURCE_DIR}/convenience"
                          )