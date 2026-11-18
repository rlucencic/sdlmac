# sdlmac: the Macintosh Plus, SE and Classic with SDL 3.
#
#   cmake -S . -B build -G Ninja && cmake --build build
#   build/sdlmac --help
#
# The Mac itself is esp32mac's (../esp32mac/components/macintosh, platform
# independent), the runtime sdlemu's (../sdlemu), the 68000 (Musashi)
# esp32emu's (../esp32emu): all four cloned side by side.
cmake_minimum_required(VERSION 3.20)
project(sdlmac C)

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    set(CMAKE_BUILD_TYPE RelWithDebInfo CACHE STRING "" FORCE)
endif()

set(ESP32MAC_DIR "${CMAKE_CURRENT_LIST_DIR}/../esp32mac" CACHE PATH "esp32mac checkout: the Mac")
if(NOT EXISTS "${ESP32MAC_DIR}/components/macintosh/include/mac.h")
    message(FATAL_ERROR "No esp32mac at ${ESP32MAC_DIR}: clone github.com/rlucencic/esp32mac beside sdlmac")
endif()

add_subdirectory("${CMAKE_CURRENT_LIST_DIR}/../sdlemu" sdlemu)

file(GLOB MAC_SOURCES "${ESP32MAC_DIR}/components/macintosh/*.c")
add_executable(sdlmac
    src/main.c
    src/settings.c
    src/rom_store.c
    ${MAC_SOURCES})
target_include_directories(sdlmac PRIVATE
    src
    "${ESP32MAC_DIR}/components/macintosh/include"
    "${ESP32MAC_DIR}/components/macintosh")
sdlemu_add_cores(sdlmac musashi diskspace)
# The web page's converters: StuffIt, Compact Pro, BinHex, MacBinary to HFS
# images (esp32mac's main/web/macfiles.js, LGPL: its COPYING.LGPL)
sdlemu_embed(sdlmac "${ESP32MAC_DIR}/main/web/macfiles.js" macfiles_js)
target_link_libraries(sdlmac PRIVATE sdlemu)
target_compile_options(sdlmac PRIVATE -Wall -Wextra -Wno-unused-parameter -Wno-unused-result)

install(TARGETS sdlmac)
