cmake_minimum_required(VERSION 3.11)
cmake_policy(SET CMP0079 NEW)

include(FetchContent)

project (AACS)

include(${CMAKE_SOURCE_DIR}/cmake/TestLargeFiles.cmake)

# Tests for LFS (Large FIle Support)
GMX_TEST_LARGE_FILES(LARGE_FILES)

configure_file(
    ${CMAKE_SOURCE_DIR}/include/config.h.in
    ${CMAKE_BINARY_DIR}/include/config.h
    @ONLY
)

include_directories(include)

FetchContent_Declare(Backward
    GIT_REPOSITORY https://github.com/bombela/backward-cpp.git
    GIT_PROGRESS TRUE
    FIND_PACKAGE_ARGS CONFIG NAMES Backward
)
FetchContent_MakeAvailable(Backward)

add_subdirectory(proto)
add_subdirectory(AAServer)
add_subdirectory(AAClient)
add_subdirectory(GetEvents)
