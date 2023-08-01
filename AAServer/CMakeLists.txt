cmake_minimum_required(VERSION 3.11)
project (AAServer)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Boost 1.67 REQUIRED COMPONENTS filesystem program_options)
find_package(Threads REQUIRED)
find_package(OpenSSL REQUIRED)
find_package(fmt REQUIRED)
pkg_check_modules (gst REQUIRED IMPORTED_TARGET gstreamer-base-1.0)
pkg_check_modules (LIBUSBGX REQUIRED IMPORTED_TARGET libusbgx)
pkg_check_modules (LIBPCAP REQUIRED IMPORTED_TARGET libpcap)

add_executable(AAServer main.cpp 
    ../src/utils.cpp 
    src/ServerUtils.cpp 
    src/descriptors.cpp
    src/Library.cpp
    src/Function.cpp
    src/MassStorageFunction.cpp
    src/FfsFunction.cpp
    src/Configuration.cpp
    src/Gadget.cpp
    src/Udc.cpp
    src/Message.cpp
    src/ModeSwitcher.cpp
    src/AaCommunicator.cpp
    src/SocketClient.cpp
    src/SocketCommunicator.cpp
    src/ManualResetEvent.cpp
    src/ChannelHandler.cpp
    src/DefaultChannelHandler.cpp
    src/VideoChannelHandler.cpp
    src/InputChannelHandler.cpp
    ${BACKWARD_ENABLE}
)

target_include_directories(AAServer PRIVATE ${CMAKE_BINARY_DIR}/include)
target_include_directories(AAServer PRIVATE include)
target_include_directories(AAServer PRIVATE ${gst_INCLUDE_DIRS})

add_backward(AAServer)
target_link_libraries(AAServer Threads::Threads)
target_link_libraries(AAServer Boost::filesystem)
target_link_libraries(AAServer Boost::program_options)
target_link_libraries(AAServer PkgConfig::LIBUSBGX)
target_link_libraries(AAServer OpenSSL::SSL OpenSSL::Crypto)
target_link_libraries(AAServer PkgConfig::gst)
target_link_libraries(AAServer PkgConfig::LIBPCAP)
target_link_libraries(AAServer fmt::fmt)
target_link_libraries(AAServer proto-objects)
include_directories(${LIBUSBGX_INCLUDE_DIRS})
include_directories(${LIBPCAP_INCLUDE_DIRS})

add_custom_command(OUTPUT dhparam.pem COMMAND openssl dhparam -out dhparam.pem 2048 > /dev/null 2>&1)
add_custom_target(dhparam DEPENDS dhparam.pem)
add_dependencies(AAServer dhparam)
add_custom_command(TARGET AAServer POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy
            ${CMAKE_CURRENT_SOURCE_DIR}/ssl/android_auto.crt
            ${CMAKE_CURRENT_BINARY_DIR}/android_auto.crt)
add_custom_command(TARGET AAServer POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy
            ${CMAKE_CURRENT_SOURCE_DIR}/ssl/android_auto.key
            ${CMAKE_CURRENT_BINARY_DIR}/android_auto.key)
