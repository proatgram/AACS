add_library(proto-objects
    OBJECT
        "${CMAKE_CURRENT_LIST_DIR}/MediaStreamType.proto"
        "${CMAKE_CURRENT_LIST_DIR}/MediaChannel.proto"
        "${CMAKE_CURRENT_LIST_DIR}/MediaChannelSetupResponse.proto"
        "${CMAKE_CURRENT_LIST_DIR}/Channel.proto"
        "${CMAKE_CURRENT_LIST_DIR}/ChannelOpenRequest.proto"
        "${CMAKE_CURRENT_LIST_DIR}/ServiceDiscoveryRequest.proto"
        "${CMAKE_CURRENT_LIST_DIR}/ServiceDiscoveryResponse.proto"
        "${CMAKE_CURRENT_LIST_DIR}/InputChannel.proto"
        "${CMAKE_CURRENT_LIST_DIR}/TouchConfig.proto"
        "${CMAKE_CURRENT_LIST_DIR}/SensorChannel.proto"
        "${CMAKE_CURRENT_LIST_DIR}/Sensor.proto"
        "${CMAKE_CURRENT_LIST_DIR}/VendorExtensionChannel.proto"
        "${CMAKE_CURRENT_LIST_DIR}/NavigationChannel.proto"
        "${CMAKE_CURRENT_LIST_DIR}/MediaInputChannel.proto"
        "${CMAKE_CURRENT_LIST_DIR}/BluetoothChannel.proto"
        "${CMAKE_CURRENT_LIST_DIR}/AudioConfig.proto"
        "${CMAKE_CURRENT_LIST_DIR}/InputEvent.proto"
        "${CMAKE_CURRENT_LIST_DIR}/TouchEvent.proto"
        "${CMAKE_CURRENT_LIST_DIR}/ButtonsEvent.proto"
        "${CMAKE_CURRENT_LIST_DIR}/TouchLocation.proto"
        "${CMAKE_CURRENT_LIST_DIR}/TouchAction.proto"
        "${CMAKE_CURRENT_LIST_DIR}/AudioType.proto"
        "${CMAKE_CURRENT_LIST_DIR}/VideoConfig.proto"
        "${CMAKE_CURRENT_LIST_DIR}/VideoResolution.proto"
        "${CMAKE_CURRENT_LIST_DIR}/VideoFps.proto"
        "${CMAKE_CURRENT_LIST_DIR}/PingRequest.proto"
        "${CMAKE_CURRENT_LIST_DIR}/PingResponse.proto"
    )
find_package(Protobuf REQUIRED CONFIG)

target_link_libraries(proto-objects PUBLIC protobuf::libprotobuf)

set(PROTO_BINARY_DIR "${CMAKE_CURRENT_BINARY_DIR}")
set(PROTO_IMPORT_DIRS "${CMAKE_CURRENT_LIST_DIR}")

target_include_directories(proto-objects PUBLIC "$<BUILD_INTERFACE:${PROTO_BINARY_DIR}>")

protobuf_generate(
    TARGET proto-objects
    IMPORT_DIRS ${PROTO_IMPORT_DIRS}
    PROTOC_OUT_DIR "${PROTO_BINARY_DIR}"
    )

protobuf_generate(
    TARGET proto-objects
    LANGUAGE cpp
    GENERATE_EXTENTIONS .pb.h .pb.cc
    # IMPORT_DIRS "${PROTO_IMPORT_DIRS}"
    PROTOC_OUT_DIR "${PROTO_BINARY_DIR}"
    )
