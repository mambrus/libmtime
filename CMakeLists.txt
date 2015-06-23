INCLUDE (CheckLibraryExists)
include_directories("${PROJECT_BINARY_DIR}")
include_directories("${PROJECT_SOURCE_DIR}")

set(LIBTIME_SOURCE
	time.c initfini.c
)

set(CMAKE_MODULE_LINKER_FLAGS "${CMAKE_MODULE_LINKER_FLAGS} -Wl,--undefined=__mtime_init")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -Wl,--undefined=__mtime_init")

set(LIBTIME_SOURCE
	${LIBTIME_SOURCE}
)

add_library(libmtime ${LIBTIME_SOURCE})

CHECK_LIBRARY_EXISTS(rt clock_gettime "" HAVE_LIB_RT)
if (HAVE_LIB_RT)
    target_link_libraries(libmtime rt)
else (HAVE_LIB_RT)
    target_link_libraries(libmtime)
endif (HAVE_LIB_RT)
