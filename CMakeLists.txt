cmake_minimum_required(VERSION 3.5)
project(webrtc-checkout)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH}
    ${CMAKE_SOURCE_DIR}/CMakeModules)

find_package(Git REQUIRED)
find_package(PythonInterp 2.7 REQUIRED)

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

  set(DEPOT_TOOLS_PATH ${CMAKE_BINARY_DIR}/depot_tools/src/depot-tools)
endif (NOT HAS_OWN_DEPOT_TOOLS)

list(APPEND _WEBRTC_PATH ${DEPOT_TOOLS_PATH} $ENV{PATH})
if (WIN32)
  string(REGEX REPLACE "/" "\\\\" _WEBRTC_PATH "${_WEBRTC_PATH}")
  string(REGEX REPLACE ";" "\\\\\\\;" _WEBRTC_PATH "${_WEBRTC_PATH}")
else (WIN32)
  string(REGEX REPLACE ";" ":" _WEBRTC_PATH "${_WEBRTC_PATH}")
endif (WIN32)

set(_ENV
    PATH="${_WEBRTC_PATH}"
)

set(PREFIX_EXECUTE ${CMAKE_COMMAND} -E env "${_ENV}")

 
function(libwebrtc_command)
  set(ONE_VALUE_ARGS NAME COMMENT WORKING_DIRECTORY)
  set(MULTI_VALUE_ARGS COMMAND DEPENDS)
  cmake_parse_arguments(COMMAND "" "${ONE_VALUE_ARGS}" "${MULTI_VALUE_ARGS}" ${ARGN} )

  set(CMF_DIR ${CMAKE_BINARY_DIR}${CMAKE_FILES_DIRECTORY})
  set(STAMP_FILE "${CMF_DIR}/${COMMAND_NAME}-complete")

  add_custom_command(
      OUTPUT ${STAMP_FILE}
      COMMENT ${COMMAND_COMMENT}
      COMMAND ${PREFIX_EXECUTE} ${COMMAND_COMMAND}
      COMMAND ${CMAKE_COMMAND} -E touch ${STAMP_FILE}
      WORKING_DIRECTORY ${COMMAND_WORKING_DIRECTORY}
  )

  add_custom_target(${COMMAND_NAME} ALL DEPENDS ${STAMP_FILE})

  if (COMMAND_DEPENDS)
    add_dependencies(${COMMAND_NAME} ${COMMAND_DEPENDS})
  endif (COMMAND_DEPENDS)
endfunction()

libwebrtc_command(
    NAME webrtc-sync
    COMMAND fetch --nohooks webrtc 
    WORKING_DIRECTORY "${CMAKE_BINARY_DIR}"
    COMMENT "Synchronizing WebRTC"
)
