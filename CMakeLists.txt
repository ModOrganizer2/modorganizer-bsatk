cmake_minimum_required(VERSION 3.16)

project(bsatk)
set(project_type lib)
set(enable_warnings OFF)
set(enable_permissive ON)
set(create_translations OFF)

if(DEFINED DEPENDENCIES_DIR)
	include(${DEPENDENCIES_DIR}/modorganizer_super/cmake_common/project.cmake)
else()
	include(../cmake_common/project.cmake)
endif()
add_subdirectory(src)
