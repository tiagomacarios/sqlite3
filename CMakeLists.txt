cmake_minimum_required(VERSION 3.0)
project(sqlite3)

set(SQLITE3_HEADERS
    sqlite3.h
)

set(SQLITE3_SRC
    sqlite3.c
)

add_definitions(
    -DSQLITE_THREADSAFE=1
    -DSQLITE_ENABLE_FTS3=1
    -DSQLITE_ENABLE_RTREE=1
    -DSQLITE_ENABLE_COLUMN_METADATA=1
    -DSQLITE_ENABLE_UNLOCK_NOTIFY=1
)

if(BUILD_SHARED_LIBS AND WIN32)
    add_definitions("-DSQLITE_API=__declspec(dllexport)")
endif()

IF (UNIX)
    find_package(Threads REQUIRED)

    set(SQLITE3_LIBRARIES
        Threads::Threads
        dl
    )
ENDIF()

add_library(sqlite3 ${SQLITE3_HEADERS} ${SQLITE3_SRC})
target_link_libraries (sqlite3 ${SQLITE3_LIBRARIES})
target_include_directories(sqlite3 PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
