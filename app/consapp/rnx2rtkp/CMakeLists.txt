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

# set (INPUT11 ${CMAKE_CURRENT_SOURCE_DIR}/../../../test/data/rinex/07590920.05o " " ${CMAKE_CURRENT_SOURCE_DIR}/../../../test/data/rinex/30400920.05n)
set (INPUT11 ${CMAKE_CURRENT_SOURCE_DIR}\\..\\..\\..\\test\\data\\rinex\\07590920.05o " " ${CMAKE_CURRENT_SOURCE_DIR}\\..\\..\\..\\test\\data\\rinex\\30400920.05n)

set (INPUT12 ${CMAKE_CURRENT_SOURCE_DIR}\\..\\..\\..\\test\\data\\rinex\\30400920.05o)
set (OPTS1 -r -3978241.958 3382840.234 3649900.853)
set (CONF_FOLDER ${CMAKE_CURRENT_SOURCE_DIR}\\conf\\)

enable_testing()
add_test(NAME test1 COMMAND ${TARGET_NAME} ${INPUT11} -x 5 -o test1.pos)
add_test(NAME test2 COMMAND ${TARGET_NAME} -t -e ${OPTS1} ${INPUT11} -x -5 -o test2.pos)
add_test(NAME test3 COMMAND ${TARGET_NAME} -t -p 1 -e ${OPTS1} ${INPUT11} ${INPUT12} -o test3.pos)
add_test(NAME test4  COMMAND ${TARGET_NAME} -t -p 3 -e ${OPTS1} ${INPUT11} ${INPUT12} -o test4.pos)
add_test(NAME test5  COMMAND ${TARGET_NAME} -t -m 15 -e ${OPTS1} ${INPUT11} ${INPUT12} -o test5.pos)
add_test(NAME test6  COMMAND ${TARGET_NAME} -t -f 1 -e ${OPTS1} ${INPUT11} ${INPUT12} -o test6.pos)
add_test(NAME test7  COMMAND ${TARGET_NAME} -t -v 5 -e ${OPTS1} ${INPUT11} ${INPUT12} -o test7.pos)
add_test(NAME test8  COMMAND ${TARGET_NAME} -t -i -e ${OPTS1} ${INPUT11} ${INPUT12} -o test8.pos)
add_test(NAME test9  COMMAND ${TARGET_NAME} -t -p 0 ${OPTS1} ${INPUT11} > test9.pos)
add_test(NAME test10  COMMAND ${TARGET_NAME} -t -p 0 ${OPTS1} ${INPUT11} -o test10.pos)
add_test(NAME test11  COMMAND ${TARGET_NAME} -t -p 0 -n ${OPTS1} ${INPUT11} -o test11.pos)
add_test(NAME test12  COMMAND ${TARGET_NAME} -t -p 0 -g ${OPTS1} ${INPUT11} -o test12.pos)
add_test(NAME test13  COMMAND ${TARGET_NAME} -t -p 0 ${OPTS1} ${INPUT11} -o test13.pos)
add_test(NAME test14  COMMAND ${TARGET_NAME} -t -p 0 -u ${OPTS1} ${INPUT11} -o test14.pos)
add_test(NAME test15  COMMAND ${TARGET_NAME} -t -p 0 -d 9 ${OPTS1} ${INPUT11} -o test15.pos)
add_test(NAME test16  COMMAND ${TARGET_NAME} -t -p 0 -s , ${OPTS1} ${INPUT11} -o test16.pos)
add_test(NAME test17  COMMAND ${TARGET_NAME} -t -b -e ${OPTS1} ${INPUT11} ${INPUT12} -o test17.pos)
add_test(NAME test18  COMMAND ${TARGET_NAME} -t -c -e ${OPTS1} ${INPUT11} ${INPUT12} -o test18.pos)
add_test(NAME test19  COMMAND ${TARGET_NAME} -t -h -e ${OPTS1} ${INPUT11} ${INPUT12} -o test19.pos)
add_test(NAME test20  COMMAND ${TARGET_NAME} -t -p 4 -a ${OPTS1} ${INPUT11} ${INPUT12} -o test20.pos)
add_test(NAME test21  COMMAND ${TARGET_NAME} ${INPUT11} ${INPUT12} -o test21.pos)
# add_test(NAME test22  COMMAND ${TARGET_NAME} -k ${CONF_FOLDER}opts1.conf ${INPUT11} ${INPUT12}-o test22.pos)
# add_test(NAME test23  COMMAND ${TARGET_NAME} -k ${CONF_FOLDER}opts2.conf ${INPUT11} ${INPUT12}-o test23.pos)
# add_test(NAME test24  COMMAND ${TARGET_NAME} -k ${CONF_FOLDER}opts3.conf ${INPUT11} ${INPUT12} -y 2 -o test24.pos)
# add_test(NAME test25  COMMAND ${TARGET_NAME} -k ${CONF_FOLDER}opts4.conf ${INPUT11} ${INPUT12} -y 2 -o test25.pos)