# == DO NOT EDIT THE FOLLOWING LINES for the Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.1.1)
set(toolchainVersion 14_2_Rel1)
set(picotoolVersion 2.1.1)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# ====================================================================================
set(PICO_BOARD pico2 CACHE STRING "Board type")

cmake_minimum_required(VERSION 3.12)

# Pull in SDK (must be before project)
include(pico_sdk_import.cmake)

project(pegsolpico C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Initialize the SDK
pico_sdk_init()

add_executable(pegsolpico
	pegsolitaire.c
	ili9341_spi.c
	graphlib.c
	graphlib.h
	LCDdriver.h
)

# Pull in basic dependencies
target_link_libraries(pegsolpico pico_stdlib hardware_spi hardware_pwm)

# create map/bin/hex file etc.
pico_add_extra_outputs(pegsolpico)

target_link_libraries(pegsolpico jxglib_USBHost)
target_sources(pegsolpico PRIVATE jxglib_adapter.cpp)
add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/../pico-jxglib pico-jxglib)
jxglib_configure_USBHost(pegsolpico CFG_TUH_HID 3)
