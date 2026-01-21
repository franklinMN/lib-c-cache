cmake_minimum_required(VERSION 3.15)

project(
    ccache
    VERSION 0.1.0
    LANGUAGES C
)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

add_subdirectory(third_party/libds)

add_library(ccache
    src/ccache.c
)

include(CMakePackageConfigHelpers)

install(
    TARGETS ccache
    EXPORT ccacheTargets
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

install(
    DIRECTORY include/
    DESTINATION include
)

install(
    EXPORT ccacheTargets
    FILE ccacheTargets.cmake
    NAMESPACE ccache::
    DESTINATION lib/cmake/ccache
)

configure_package_config_file(
    cmake/ccacheConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/ccacheConfig.cmake
    INSTALL_DESTINATION lib/cmake/ccache
)

install(
    FILES ${CMAKE_CURRENT_BINARY_DIR}/ccacheConfig.cmake
    DESTINATION lib/cmake/ccache
)

target_include_directories(ccache
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/src
)

target_link_libraries(ccache
    PRIVATE
        libds
)

