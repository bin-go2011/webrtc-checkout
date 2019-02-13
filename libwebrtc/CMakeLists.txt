cmake_minimum_required(VERSION 3.5)
project(libwebrtc)

include(LibWebRTCCommand)
include(Gn)

libwebrtc_command(
    NAME webrtc-generate
    COMMAND ${_GEN_COMMAND} gen out/Default
    WORKING_DIRECTORY "${WEBRTC_ROOT_PATH}"
    COMMENT "Generating build files"
    DEPENDS webrtc-sync
)
