project(system_blocks)

cmake_minimum_required(VERSION 2.4.0)

if(COMMAND cmake_policy)
    cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -nostdinc")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -nostdlib -Wl,-dylib_install_name,/usr/lib/system/libsystem_blocks.dylib -Wl,-flat_namespace -Wl,-undefined,suppress")

add_definitions(-DTARGET_OS_MAC=1)
add_definitions(-DHAVE_STDINT_H=1)
add_definitions(-D__APPLE__ -D__DYNAMIC__)

set(blocks_sources
	data.c
	runtime.c
)

add_darling_library(system_blocks SHARED ${blocks_sources})
target_link_libraries(system_blocks PRIVATE system_c system_kernel system_m system_malloc)

install(TARGETS system_blocks DESTINATION ${CMAKE_INSTALL_LIBDIR}/darling)
