cmake_minimum_required(VERSION 3.5)
project(kptools)

include_directories(${CMAKE_CURRENT_BINARY_DIR})

set(SOURCES
	image.c
	kallsym.c
	kptools.c
	order.c
	insn.c
	patch.c
	symbol.c
	kpm.c
	common.c
	sha256.c
	bootimg.c
)

add_executable(kptools ${SOURCES})

find_package(ZLIB REQUIRED)

if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -static")
    # 显式指定静态库路径
    target_link_libraries(kptools PRIVATE /usr/lib/x86_64-linux-gnu/libz.a)
elseif(CMAKE_SYSTEM_NAME STREQUAL "Android")
    target_link_libraries(kptools PRIVATE ${ZLIB_LIBRARIES})
else()
    target_link_libraries(kptools PRIVATE ${ZLIB_LIBRARIES})
endif()

target_include_directories(kptools PRIVATE ${ZLIB_INCLUDE_DIRS})