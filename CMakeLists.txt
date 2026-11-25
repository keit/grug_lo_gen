cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# == DO NOT EDIT THE FOLLOWING LINES for the Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.3.1)
set(toolchainVersion 15_2_Rel1)
set(picotoolVersion 2.3.1)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# ====================================================================================
set(PICO_BOARD pico CACHE STRING "Board type")

# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)

# Project name matches the executable name so the VS Code extension
# finds build/grug_lo_gen_pio.elf for Run / Flash / Debug.
project(grug_lo_gen_pio C CXX ASM)

pico_sdk_init()

add_executable(grug_lo_gen_pio)

# enable usb output, disable uart output
pico_enable_stdio_usb(grug_lo_gen_pio 1)
pico_enable_stdio_uart(grug_lo_gen_pio 0)

pico_generate_pio_header(grug_lo_gen_pio ${CMAKE_CURRENT_LIST_DIR}/grug_lo_gen.pio)

target_sources(grug_lo_gen_pio PRIVATE grug_lo_gen.c)

target_link_libraries(grug_lo_gen_pio PRIVATE
        pico_stdlib
        hardware_pio)

pico_add_extra_outputs(grug_lo_gen_pio)
