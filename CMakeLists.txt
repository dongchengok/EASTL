#-------------------------------------------------------------------------------------------
# Copyright (C) Electronic Arts Inc.  All rights reserved.
#-------------------------------------------------------------------------------------------
cmake_minimum_required(VERSION 3.1)
project(EASTL CXX)

#-------------------------------------------------------------------------------------------
# Options
#-------------------------------------------------------------------------------------------
option(EASTL_BUILD_BENCHMARK "Enable generation of build files for benchmark" OFF)
option(EASTL_BUILD_TESTS "Enable generation of build files for tests" OFF)

#-------------------------------------------------------------------------------------------
# Compiler Flags
#-------------------------------------------------------------------------------------------
set (CMAKE_MODULE_PATH "${CMAKE_MODULE_PATH};${CMAKE_CURRENT_SOURCE_DIR}/scripts/CMake")
include(CommonCppFlags)

#-------------------------------------------------------------------------------------------
# Library definition
#-------------------------------------------------------------------------------------------
file(GLOB EASTL_SOURCES "source/*.cpp")
add_library(EASTL STATIC ${EASTL_SOURCES})

if(EASTL_BUILD_BENCHMARK)
    add_subdirectory(benchmark)
endif()

if(EASTL_BUILD_TESTS)
    add_subdirectory(test)
endif()

#-------------------------------------------------------------------------------------------
# Defines
#-------------------------------------------------------------------------------------------
add_definitions(-D_CHAR16T)
add_definitions(-D_CRT_SECURE_NO_WARNINGS)
add_definitions(-D_SCL_SECURE_NO_WARNINGS)
add_definitions(-DEASTL_OPENSOURCE=1)

#-------------------------------------------------------------------------------------------
# Include dirs
#-------------------------------------------------------------------------------------------
target_include_directories(EASTL PUBLIC include)

#-------------------------------------------------------------------------------------------
# Dependencies
#-------------------------------------------------------------------------------------------
if (NOT TARGET EABase)
    add_subdirectory(test/packages/EABase)
endif()

target_link_libraries(EASTL EABase)

# if(BUILD_SHARED_LIBS)
#   target_compile_definitions(EASTL PUBLIC EA_DLL)
#   if(MSVC OR CYGWIN)
#     target_compile_definitions(EASTL PRIVATE "EASTL_API=__declspec(dllexport)")
#   endif()
# endif()

# 接入EASTL的VS调试模板
# https://eastl.docsforge.com/#projects-and-products-using-eastl
add_custom_target(NatVis SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/doc/EASTL.natvis)

set_target_properties(EASTL  PROPERTIES FOLDER ${JOKER_NAME_3RD})
set_target_properties(NatVis  PROPERTIES FOLDER ${JOKER_NAME_3RD})
