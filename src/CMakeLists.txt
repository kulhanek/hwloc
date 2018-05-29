# ==============================================================================
# HWLOC versions

SET(VERSION 2.0.1)

# ==============================================================================

IF(NOT EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/hwloc-${VERSION}")
    EXEC_PROGRAM(${CMAKE_COMMAND} ${CMAKE_CURRENT_SOURCE_DIR} ARGS -E tar xvf hwloc-${VERSION}.tar.gz)
ENDIF()

# ==============================================================================

IF(NOT EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/build")
    EXEC_PROGRAM(env "${CMAKE_CURRENT_SOURCE_DIR}/build" ARGS CFLAGS="-fPIC" ${CMAKE_CURRENT_SOURCE_DIR}/hwloc-${VERSION}/configure --prefix=\"${CMAKE_SOURCE_DIR}\" --enable-static --disable-shared --disable-libxml2 --disable-cairo --without-x )
ENDIF()

INCLUDE(ProcessorCount)
ProcessorCount(N)
MESSAGE(">>> Number of CPUs: ${N}") 
IF(NOT N EQUAL 0)
    ADD_CUSTOM_TARGET(hwloc ALL make install -j${N} WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/build")
ELSE()
    ADD_CUSTOM_TARGET(hwloc ALL make install WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/build")
ENDIF()

# FIXME
#INSTALL(CODE "exec_program(make install \"${CMAKE_CURRENT_SOURCE_DIR}/build\" ARGS install)")

