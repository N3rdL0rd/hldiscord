cmake_minimum_required(VERSION 3.10)

project(discord)

set(HASHLINK_SRC ${CMAKE_SOURCE_DIR}/../hlmod-hl)

add_library(discord SHARED src/hldiscord.cpp)
set_target_properties(discord PROPERTIES SUFFIX ".hdll")

target_include_directories(discord PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/include"
    "${CMAKE_CURRENT_SOURCE_DIR}/includes"
    "${HASHLINK_SRC}/src"
)

find_library(HL_LIBRARY NAMES libhl PATHS "${HASHLINK_SRC}/build/bin")
find_library(DISCORD_RPC_LIBRARY NAMES discord-rpc PATHS "${CMAKE_CURRENT_SOURCE_DIR}/lib")

if (HL_LIBRARY)
    target_link_libraries(discord ${HL_LIBRARY})
else()
    message(WARNING "libhl.lib not found. Please build hlmod-hl project first.")
endif()

if (DISCORD_RPC_LIBRARY)
    target_link_libraries(discord ${DISCORD_RPC_LIBRARY})
else()
    message(WARNING "discord-rpc.lib not found. Please make sure it is in the lib directory.")
endif()

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
