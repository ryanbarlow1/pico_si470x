# == DO NOT EDIT THE FOLLOWING LINES for the Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.1.0)
set(toolchainVersion 13_3_Rel1)
set(picotoolVersion 2.1.0)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# ====================================================================================
set(PICO_BOARD pico CACHE STRING "Board type")

cmake_minimum_required(VERSION 3.13)

include(pico_sdk_import.cmake)

project(fm_project C CXX ASM)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

pico_sdk_init()

add_subdirectory(fm_si470x)
add_subdirectory(rds_parser)

add_executable(fm_example fm_example.c)

pico_enable_stdio_uart(fm_example 1)
pico_enable_stdio_usb(fm_example 1)

pico_add_extra_outputs(fm_example)

target_compile_options(fm_example PRIVATE -Wall -Wextra)

target_link_libraries(fm_example fm_si470x rds_parser pico_stdlib)
