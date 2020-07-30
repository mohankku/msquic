# Copyright (c) Microsoft Corporation.
# Licensed under the MIT License.

set(SOURCES
    quicmain.cpp
    ThroughputServer.cpp
    ThroughputClient.cpp
)

# Allow CLOG to preprocess all the source files.
add_clog_library(perflib.clog STATIC ${SOURCES})

add_library(perflib ${SOURCES})

set_property(TARGET perflib PROPERTY FOLDER "perf")

target_link_libraries(perflib PUBLIC perflib.clog)

target_link_libraries(perflib PRIVATE inc warnings)

target_include_directories(perflib PUBLIC ${CMAKE_CURRENT_LIST_DIR})

if (MSVC)
    target_compile_options(perflib PUBLIC /wd4459)
else()
    target_compile_options(perflib PUBLIC -Wno-switch)
endif()

if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "MSVC")
    add_dependencies(perflib MsQuicEtw)
endif()