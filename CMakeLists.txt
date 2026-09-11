cmake_minimum_required(VERSION 3.20)

project(mp3_player
    VERSION 0.1.0
    LANGUAGES CXX
)

find_package(Threads REQUIRED)

add_library(miniaudio STATIC
    external/miniaudio/miniaudio.cpp
)

target_include_directories(miniaudio SYSTEM PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/external/miniaudio"
)

target_link_libraries(miniaudio PUBLIC
    Threads::Threads
    ${CMAKE_DL_LIBS}
)

if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    target_link_libraries(miniaudio PUBLIC m)
endif()

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_executable(mp3_player
    src/main.cpp
)

target_link_libraries(mp3_player PRIVATE miniaudio)

target_include_directories(mp3_player
    PRIVATE include
)

target_compile_options(mp3_player PRIVATE
    -Wall
    -Wextra
    -Wpedantic
)