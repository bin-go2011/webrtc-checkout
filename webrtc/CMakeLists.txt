cmake_minimum_required(VERSION 3.5)
project(webrtc)

message(STATUS ${CMAKE_BINARY_DIR})
#set(WEBRTC_ROOT_PATH "" CACHE STRING "Path to your own webrtc directory")

include(LibWebRTCCommand)

libwebrtc_command(
    NAME webrtc-fetch
    COMMAND fetch --nohooks webrtc 
    WORKING_DIRECTORY "${CMAKE_BINARY_DIR}"
    COMMENT "Fetching WebRTC"
)
#    set(WEBRTC_ROOT_PATH ${CMAKE_BINARY_DIR})

libwebrtc_command(
    NAME webrtc-sync
    COMMAND gclient sync
    WORKING_DIRECTORY "${CMAKE_BINARY_DIR}"
    COMMENT "Synchronizing WebRTC"
    DEPENDS webrtc-fetch
)
