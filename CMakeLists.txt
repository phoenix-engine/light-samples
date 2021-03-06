# Light Samples

########################################################################
# CMake policies
########################################################################

cmake_minimum_required(VERSION 3.1.0 FATAL_ERROR)

# project() command manages VERSION variables.
if(POLICY CMP0048)
  cmake_policy(SET CMP0048 NEW)
endif() # CMP0048

# OLD: target_sources() command converts relative paths to absolute.
if(POLICY CMP0076)
  cmake_policy(SET CMP0076 NEW)
endif(POLICY CMP0076)

# OLD: Targets need to be created in the same directory to be used.
if(POLICY CMP0079)
  cmake_policy(SET CMP0079 NEW)
endif(POLICY CMP0079)

########################################################################
# CMake Project
########################################################################
project(LightSamples LANGUAGES C CXX)

cmake_minimum_required(VERSION 3.1.0 FATAL_ERROR)

# if(WIN32)
#   set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)
# endif(WIN32)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Set application to GUI type for Win32 and MacOS
if(WIN32 AND NOT(CMAKE_BUILD_TYPE STREQUAL "Debug"))
  set(GUI_TYPE WIN32)
endif() # WIN32
if(APPLE)
  set(GUI_TYPE MACOSX_BUNDLE)
endif(APPLE)

# Look in this path for CMake modules, before ${CMAKE_ROOT}/Modules/ is
# checked.
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/Modules)

# disallow in-source build
include(MacroEnsureOutOfSourceBuild)
macro_ensure_out_of_source_build(
  "${PROJECT_NAME} requires an out of "
  "source build. Please create a separate build directory and run "
  "`cmake /path/to/${PROJECT_NAME} [options]` there."
)

# global variables
set(Sample_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/include)
set(Sample_TARGET_DIR  ${CMAKE_BINARY_DIR}/target)
set(Sample_STATIC_LIB_DIR ${CMAKE_BINARY_DIR}/lib)

# Shared libs should be shipped with the target executable.
set(Sample_SHARED_LIB_DIR ${Sample_TARGET_DIR}/lib)

# Define output directories
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${Sample_TARGET_DIR})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${Sample_SHARED_LIB_DIR})
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${Sample_STATIC_LIB_DIR})

add_subdirectory(dep)
add_subdirectory(basic-triangle)

# If the user didn't set the build type, tell them debugging may fail.
if(NOT(CMAKE_BUILD_TYPE))
  message(
    WARNING
    "Build type was not set.  To export debugging symbols, set the "
    "build type with `-DCMAKE_BUILD_TYPE=Debug`."
  )
endif(NOT(CMAKE_BUILD_TYPE))
