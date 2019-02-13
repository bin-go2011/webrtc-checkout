cmake_minimum_required(VERSION 3.5)
project(libwebrtc)

include(Gn)

libwebrtc_command(
    NAME webrtc-generate
    COMMAND ${_GEN_COMMAND}
    WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}"
    COMMENT "Generating build files"
    DEPENDS webrtc-sync
)
