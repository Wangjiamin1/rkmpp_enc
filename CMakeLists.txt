# ----------------------------------------------------------------------------
#  Root CMake file for Rockchip Media Process Platform (MPP)
#
#   - 10:34 2015/7/27: Initial version <herman.chen@rock-chips.com>
#
# ----------------------------------------------------------------------------

# vim: syntax=cmake
if(NOT CMAKE_BUILD_TYPE)
    # default to Release build for GCC builds
    set(CMAKE_BUILD_TYPE Debug CACHE STRING
        "Choose the type of build, options are: None(CMAKE_CXX_FLAGS or CMAKE_C_FLAGS used) Debug Release RelWithDebInfo MinSizeRel."
        FORCE)
endif()
message(STATUS "cmake version ${CMAKE_VERSION}")

cmake_minimum_required (VERSION 2.8.8) # OBJECT libraries require 2.8.8
include(CheckIncludeFiles)
include(CheckFunctionExists)
include(CheckSymbolExists)
include(CheckCXXCompilerFlag)

# Search packages for host system instead of packages for target system
# in case of cross compilation these macro should be defined by toolchain file
if(NOT COMMAND find_host_package)
    macro(find_host_package)
        find_package(${ARGN})
    endmacro()
endif()
if(NOT COMMAND find_host_program)
    macro(find_host_program)
        find_program(${ARGN})
    endmacro()
endif()

project (rk_mpp)

# ----------------------------------------------------------------------------
# enable test in this project
# ----------------------------------------------------------------------------

include_directories(${PROJECT_SOURCE_DIR}/inc)
include_directories(${PROJECT_SOURCE_DIR}/common)
include_directories(${PROJECT_SOURCE_DIR}/base/inc)
include_directories(${PROJECT_SOURCE_DIR}/codec/inc)
include_directories(${PROJECT_SOURCE_DIR}/hal/inc)
include_directories(${PROJECT_SOURCE_DIR}/hal/common)
include_directories(${PROJECT_SOURCE_DIR}/vproc/inc)
include_directories(${PROJECT_SOURCE_DIR}/include)


link_directories(/home/rpdzkj/env/mpp/build/linux/aarch64/utils)




set(module mpi_enc)

set(ext c)
set(test_name ${module}_test)
set(file_name ${test_name}.${ext})
string(TOUPPER ${test_name} test_tag)
if (NOT MPP_PROJECT_NAME)
    set(MPP_PROJECT_NAME rockchip_mpp)
endif()
set(MPP_SHARED ${MPP_PROJECT_NAME})

set(CMAKE_INSTALL_PREFIX "./install/")

add_executable(${test_name} ${file_name} mpp_event_trigger.c mpp_parse_cfg.c)
target_link_libraries(${test_name} ${MPP_SHARED} utils)