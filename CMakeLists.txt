cmake_minimum_required(VERSION 3.10)
project(webrtc-checkout)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH}
    ${CMAKE_SOURCE_DIR}/CMakeModules)

find_package(Git REQUIRED)
find_package(PythonInterp 2.7 REQUIRED)

include(ExternalProject)
if (NOT HAS_OWN_DEPOT_TOOLS)
  if (WIN32)
    set(GCLIENT_EXECUTABLE ${CMAKE_BINARY_DIR}/depot_tools/src/depot-tools/gclient.bat)
  else (WIN32)
    set(GCLIENT_EXECUTABLE ${CMAKE_BINARY_DIR}/depot_tools/src/depot-tools/gclient)
  endif (WIN32)

  ExternalProject_Add(
      depot-tools

      GIT_REPOSITORY https://chromium.googlesource.com/chromium/tools/depot_tools

      PREFIX ${CMAKE_BINARY_DIR}/depot_tools

      CONFIGURE_COMMAND ""
      UPDATE_COMMAND ""
      PATCH_COMMAND ""
      BUILD_COMMAND ""
      INSTALL_COMMAND ""
  )

  set(DEPOT_TOOLS_PATH ${CMAKE_BINARY_DIR}/depot_tools/src/depot-tools)
endif (NOT HAS_OWN_DEPOT_TOOLS)

message(STATUS ${GCLIENT_EXECUTABLE})
add_custom_target(
  ALL
  DEPENDS depot-tools
  COMMAND ${GCLIENT_EXECUTABLE} sync
  WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
)