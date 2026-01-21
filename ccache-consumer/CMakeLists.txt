cmake_minimum_required(VERSION 3.15)
project(ccache_consumer C)
set(CMAKE_C_STANDARD 11)

find_package(ccache REQUIRED)

add_executable(app main.c)
target_link_libraries(app PRIVATE ccache::ccache)