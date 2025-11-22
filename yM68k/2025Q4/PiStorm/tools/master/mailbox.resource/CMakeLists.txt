cmake_minimum_required(VERSION 3.14.0)
project(mailbox VERSION 1.0.0)
get_verstring(VERSTRING)

set_source_files_properties(src/main.c PROPERTIES COMPILE_FLAGS -mno-pcrel)

add_library(mailbox INTERFACE)

add_executable(mailbox.resource
    src/main.c
    src/init.c
    src/rawcommand.c
    src/stringcommand.c
    src/getclockrate.c
    src/setclockrate.c
    src/getclockstate.c
    src/setclockstate.c
    src/getpowerstate.c
    src/setpowerstate.c
    src/getgpiostate.c
    src/setgpiostate.c
    src/mbox.c
)

target_link_options(mailbox.resource PRIVATE -ffreestanding -nostdlib -nostartfiles -s -Wl,-e_code_start -Wl,-T${CMAKE_CURRENT_SOURCE_DIR}/ldscript.lds)
target_compile_options(mailbox.resource PRIVATE -Os -m68040 -msmall-code -mpcrel -fomit-frame-pointer)
target_compile_definitions(mailbox.resource PRIVATE VERSION_STRING="${VERSTRING}")

target_include_directories(mailbox.resource PRIVATE include)
target_include_directories(mailbox INTERFACE include)

target_link_libraries(mailbox.resource common devicetree)

set_target_properties(mailbox.resource PROPERTIES LINK_DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/ldscript.lds)
