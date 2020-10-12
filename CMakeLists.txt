cmake_minimum_required(VERSION 3.14)
project(
  PATHWrapper
  VERSION 1.0.0
  LANGUAGES CXX)


# CXX Options
option(CXX "enable C++ compilation" ON)
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release ... FORCE)
endif()

find_library(
  PATH_LIBRARY
  NAMES path path47 path50
  HINTS ${CMAKE_CURRENT_SOURCE_DIR}/lib
  PATH_SUFFIXES lib)

find_library(
  LUSOL_LIBRARY
  NAMES lusol
  HINTS ${CMAKE_CURRENT_SOURCE_DIR}/lib
  PATH_SUFFIXES lib)

add_library(PATHWrapper STATIC PathLCP.cpp)
message("PathLIB is located at ${PATH_LIBRARY}")

target_include_directories(
  PATHWrapper
  PUBLIC include
  PRIVATE src)

target_link_libraries(PATHWrapper PUBLIC ${LUSOL_LIBRARY} ${PATH_LIBRARY} m)

set_target_properties(
  PATHWrapper
  PROPERTIES POSITION_INDEPENDENT_CODE ON CXX_VISIBILITY_PRESET hidden
             VISIBILITY_INLINES_HIDDEN ON)
