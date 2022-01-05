cmake_minimum_required(VERSION 3.21.1)

project(rnx2rtkp)

set (RTKLIB_SRC ${CMAKE_CURRENT_SOURCE_DIR}/../../../src)
include_directories(${RTKLIB_SRC})

set (SRC
    ${CMAKE_CURRENT_SOURCE_DIR}/rnx2rtkp.c
    ${RTKLIB_SRC}/rtkcmn.c
    ${RTKLIB_SRC}/rinex.c
    ${RTKLIB_SRC}/rtkpos.c
    ${RTKLIB_SRC}/postpos.c
    ${RTKLIB_SRC}/solution.c
    ${RTKLIB_SRC}/lambda.c
    ${RTKLIB_SRC}/geoid.c
    ${RTKLIB_SRC}/sbas.c
    ${RTKLIB_SRC}/preceph.c
    ${RTKLIB_SRC}/pntpos.c
    ${RTKLIB_SRC}/ephemeris.c
    ${RTKLIB_SRC}/options.c
    ${RTKLIB_SRC}/ppp.c
    ${RTKLIB_SRC}/ppp_ar.c
    ${RTKLIB_SRC}/rtcm.c
    ${RTKLIB_SRC}/rtcm2.c
    ${RTKLIB_SRC}/rtcm3.c
    ${RTKLIB_SRC}/rtcm3e.c
    ${RTKLIB_SRC}/ionex.c
    ${RTKLIB_SRC}/tides.c
)

set (TARGET_NAME rnx2rtkp)

add_executable(${TARGET_NAME} ${SRC})

if(WIN32)
  target_link_libraries(${TARGET_NAME} wsock32 ws2_32 winmm)
endif()

target_compile_definitions(${TARGET_NAME} PRIVATE TRACE ENAGLO ENAQZS ENAGAL ENACMP ENAIRN NFREQ=5)
target_compile_options(${TARGET_NAME} PRIVATE -Wall -O3 -ansi -pedantic -g)
# target_compile_options(${TARGET_NAME} PRIVATE -Wall -O3 -ansi -pedantic -Wno-unused-but-set-variable -g)

set (INPUT11 ${CMAKE_CURRENT_SOURCE_DIR}/../../../test/data/rinex/07590920.05o ${CMAKE_CURRENT_SOURCE_DIR}/../../../test/data/rinex/30400920.05n)
enable_testing()
add_test(NAME test1
COMMAND ${TARGET_NAME} ${CMAKE_CURRENT_SOURCE_DIR}/../../../test/data/rinex/07590920.05o ${CMAKE_CURRENT_SOURCE_DIR}/../../../test/data/rinex/30400920.05n  -x 5 -o test1.pos)