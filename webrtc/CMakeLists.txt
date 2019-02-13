cmake_minimum_required(VERSION 3.5)
project(webrtc)

message(STATUS ${CMAKE_BINARY_DIR})
set(WEBRTC_ROOT_PATH "" CACHE STRING "Path to your own webrtc directory")

include(LibWebRTCCommand)

if (WEBRTC_ROOT_PATH)
  set(HAS_OWN_WEBRTC 1)
endif(WEBRTC_ROOT_PATH)

if (NOT HAS_OWN_WEBRTC)
    libwebrtc_command(
        NAME webrtc-fetch
        COMMAND fetch --nohooks webrtc 
        WORKING_DIRECTORY "${CMAKE_BINARY_DIR}"
        COMMENT "Fetching WebRTC"
    )
    set(WEBRTC_ROOT_PATH ${CMAKE_BINARY_DIR})
endif(NOT HAS_OWN_WEBRTC)

# libwebrtc_command(
#     NAME webrtc-sync
#     COMMAND gclient sync
#     WORKING_DIRECTORY "${CMAKE_BINARY_DIR}"
#     COMMENT "Synchronizing WebRTC"
# )
