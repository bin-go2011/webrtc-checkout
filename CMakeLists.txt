cmake_minimum_required(VERSION 3.5)
project(webrtc-checkout)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH}
    ${CMAKE_SOURCE_DIR}/CMakeModules)

find_package(Git REQUIRED)
find_package(PythonInterp 2.7 REQUIRED)

include(Options)

if (HAS_OWN_DEPOT_TOOLS)
  find_package(DepotTools REQUIRED)
endif (HAS_OWN_DEPOT_TOOLS)

include(ExternalProject)
if (NOT HAS_OWN_DEPOT_TOOLS)
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

  set(_NEXT_DEPENDS depot-tools)
  set(DEPOT_TOOLS_PATH ${CMAKE_BINARY_DIR}/depot_tools/src/depot-tools)
endif (NOT HAS_OWN_DEPOT_TOOLS)

set(_WEBRTC_CMAKE_ARGS
    -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
    -DCMAKE_MODULE_PATH:PATH=${CMAKE_MODULE_PATH}
    -DDEPOT_TOOLS_PATH:PATH=${DEPOT_TOOLS_PATH}
)

ExternalProject_Add(
    webrtc-src

    DEPENDS ${_NEXT_DEPENDS}
    SOURCE_DIR ${CMAKE_SOURCE_DIR}/webrtc
    PREFIX ${CMAKE_BINARY_DIR}/webrtc
    BINARY_DIR ${CMAKE_BINARY_DIR}/webrtc/build
    STAMP_DIR ${CMAKE_BINARY_DIR}/webrtc/stamp
    TMP_DIR ${CMAKE_BINARY_DIR}/webrtc/tmp
    
    CMAKE_ARGS
    ${_WEBRTC_CMAKE_ARGS}
    INSTALL_COMMAND
    ""
)
