cmake_minimum_required(VERSION 3.5)
project(argus_camera LANGUAGES CUDA CXX)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_SOURCE_DIR}/cmake)
set(CMAKE_CXX_STANDARD 11)

find_package(TegraMM REQUIRED)
if(${TegraMM_FOUND})
  message(STATUS "Found Tegra Multimedia API")
else()
  message(STATUS "Could not find Tegra Multimedia API")
endif()

add_library(argus_camera SHARED
  src/ArgusCamera.cpp
  ${TegraMM_COMMON_SOURCES}
)

message("TegraMM_INCLUDE_DIRS")
message("${TegraMM_INCLUDE_DIRS}")
include_directories(${TegraMM_INCLUDE_DIRS})

target_link_libraries(argus_camera ${TegraMM_LIBRARIES})

set_target_properties(argus_camera PROPERTIES PUBLIC_HEADER 
  "src/ArgusCamera.hpp"
)

install(TARGETS argus_camera
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib/static
  PUBLIC_HEADER DESTINATION include/argus_camera)

