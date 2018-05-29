CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

IF(NOT EXISTS "${CMAKE_SOURCE_DIR}/lib/libhwloc.a")
    ADD_SUBDIRECTORY(src)
ELSE()
    # placebo
    ADD_CUSTOM_TARGET(hwloc-build ALL /bin/true)
ENDIF()

