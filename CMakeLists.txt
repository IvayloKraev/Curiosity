cmake_minimum_required(VERSION 3.27)

set(PICO_BOARD pico_w)

include(lib/pico-sdk/pico_sdk_init.cmake)
include(lib/FreeRTOS-Kernel/portable/ThirdParty/GCC/RP2040/FreeRTOS_Kernel_import.cmake)
include(lib/hctp/CMakeLists.txt)

project(Curiosity C CXX ASM)

pico_sdk_init()

set(CMAKE_C_STANDARD 11)

set(CURIOSITY_SOURCES
        src/wifiCtrl.c
        src/status.c
        src/socket.c
        src/execute.c
)

add_executable(Curiosity main.c ${CURIOSITY_SOURCES})

target_include_directories(Curiosity PRIVATE inc)

target_link_libraries(
        Curiosity
        pico_stdlib
        FreeRTOS-Kernel
        FreeRTOS-Kernel-Heap4
        pico_cyw43_arch_lwip_sys_freertos
        hctp
        hardware_gpio
        hardware_pwm
)

pico_enable_stdio_usb(Curiosity 1)
pico_enable_stdio_uart(Curiosity 0)

pico_add_extra_outputs(Curiosity)
