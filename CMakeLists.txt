# rpi5mac: the Macintosh Plus, SE and Classic on the Raspberry Pi 5.
#
#   cmake -S . -B build -G Ninja && cmake --build build
#   build/rpi5mac --help
#
# The Mac itself is esp32mac's (../esp32mac/components/macintosh, platform
# independent), the runtime rpi5emu's (../rpi5emu), the 68000 (Musashi)
# esp32emu's (../esp32emu): all four cloned side by side.
cmake_minimum_required(VERSION 3.20)
project(rpi5mac C)

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    set(CMAKE_BUILD_TYPE RelWithDebInfo CACHE STRING "" FORCE)
endif()

set(ESP32MAC_DIR "${CMAKE_CURRENT_LIST_DIR}/../esp32mac" CACHE PATH "esp32mac checkout: the Mac")
if(NOT EXISTS "${ESP32MAC_DIR}/components/macintosh/include/mac.h")
    message(FATAL_ERROR "No esp32mac at ${ESP32MAC_DIR}: clone github.com/rlucencic/esp32mac beside rpi5mac")
endif()

add_subdirectory("${CMAKE_CURRENT_LIST_DIR}/../rpi5emu" rpi5emu)

file(GLOB MAC_SOURCES "${ESP32MAC_DIR}/components/macintosh/*.c")
add_executable(rpi5mac
    src/main.c
    src/settings.c
    src/rom_store.c
    ${MAC_SOURCES})
target_include_directories(rpi5mac PRIVATE
    src
    "${ESP32MAC_DIR}/components/macintosh/include"
    "${ESP32MAC_DIR}/components/macintosh")
rpi5emu_add_cores(rpi5mac musashi diskspace)
# The web page's converters: StuffIt, Compact Pro, BinHex, MacBinary to HFS
# images (esp32mac's main/web/macfiles.js, LGPL: its COPYING.LGPL)
rpi5emu_embed(rpi5mac "${ESP32MAC_DIR}/main/web/macfiles.js" macfiles_js)
target_link_libraries(rpi5mac PRIVATE rpi5emu)
target_compile_options(rpi5mac PRIVATE -Wall -Wextra -Wno-unused-parameter -Wno-unused-result)

install(TARGETS rpi5mac)
