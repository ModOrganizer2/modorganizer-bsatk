cmake_minimum_required(VERSION 3.16)

project(bsatk)
set(project_type lib)
set(enable_warnings OFF)
set(enable_permissive ON)

include(../cmake_common/project.cmake)
add_subdirectory(src)
