cmake_minimum_required(VERSION 3.11)
project (GetEvents)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Threads REQUIRED)
find_package(X11 REQUIRED)

include_directories(${CMAKE_CURRENT_BINARY_DIR})
include_directories(${X11_INCLUDE_DIR})
include_directories(${X11_XTest_INCLUDE_PATH})
include_directories(include)

add_executable(GetEvents main.cpp)

target_link_libraries(GetEvents X11::X11)
target_link_libraries(GetEvents X11::Xtst)
target_link_libraries(GetEvents proto-objects)
