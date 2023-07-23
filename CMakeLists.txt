cmake_minimum_required(VERSION 3.11)
cmake_policy(SET CMP0079 NEW)

include(FetchContent)

project (AACS)
include_directories(include)

FetchContent_Declare(Backward
    GIT_REPOSITORY https://github.com/bombela/backward-cpp.git
    GIT_TAG v1.6
    GIT_PROGRESS TRUE
    FIND_PACKAGE_ARGS CONFIG NAMES Backward
)

FetchContent_MakeAvailable(Backward)

add_subdirectory(proto)

add_subdirectory(AAServer)
add_subdirectory(AAClient)
add_subdirectory(GetEvents)

# include(${CMAKE_SOURCE_DIR}/proto/CMakeLists.txt)
