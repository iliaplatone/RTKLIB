cmake_minimum_required(VERSION 2.6)

project(librtk C)

set(SOURCE_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/../../src)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake_modules/")

include(GNUInstallDirs)

# for beagleboard
#CTARGET= -mfpu=neon -mfloat-abi=softfp -ffast-math

include_directories(${SOURCE_DIRECTORY})

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DENAGLO -DENAGAL -DENAQZS -DENACMP -DTRACE -DNFREQ=3 -DNEXOBS=3")
set(CMAKE_LD_FLAGS "${CMAKE_LD_FLAGS} -Wall -O3 -ansi -pedantic -Wno-unused-but-set-variable -g")

set(LIB_INSTALL_DIR "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}")
set(LIBRTK_VERSION_MAJOR 2)
set(LIBRTK_VERSION_MINOR 4)
set(LIBRTK_VERSION_RELEASE 3)
set(RTK_VERSION "${LIBRTK_VERSION_MAJOR}.${LIBRTK_VERSION_MINOR}.${LIBRTK_VERSION_RELEASE}")
set(LIBRTK_VERSION 0x${LIBRTK_VERSION_MAJOR}${LIBRTK_VERSION_MINOR}${LIBRTK_VERSION_RELEASE})
set(RTK_SOVERSION ${LIBRTK_VERSION_MAJOR})

set(RTK_SOURCES_C
    ${SOURCE_DIRECTORY}/stream.c
    ${SOURCE_DIRECTORY}/rtkcmn.c
    ${SOURCE_DIRECTORY}/solution.c
    ${SOURCE_DIRECTORY}/sbas.c
    ${SOURCE_DIRECTORY}/geoid.c
    ${SOURCE_DIRECTORY}/rcvraw.c
    ${SOURCE_DIRECTORY}/rcv/novatel.c
    ${SOURCE_DIRECTORY}/rcv/ublox.c
    ${SOURCE_DIRECTORY}/rcv/ss2.c
    ${SOURCE_DIRECTORY}/rcv/crescent.c
    ${SOURCE_DIRECTORY}/rcv/septentrio.c
    ${SOURCE_DIRECTORY}/rcv/skytraq.c
    ${SOURCE_DIRECTORY}/rcv/javad.c
    ${SOURCE_DIRECTORY}/rcv/nvs.c
    ${SOURCE_DIRECTORY}/rcv/binex.c
    ${SOURCE_DIRECTORY}/rcv/rt17.c
    ${SOURCE_DIRECTORY}/rtcm.c
    ${SOURCE_DIRECTORY}/rtcm2.c
    ${SOURCE_DIRECTORY}/rtcm3.c
    ${SOURCE_DIRECTORY}/rtcm3e.c
    ${SOURCE_DIRECTORY}/preceph.c
    ${SOURCE_DIRECTORY}/streamsvr.c
    ${SOURCE_DIRECTORY}/rtklib.h
    ${CMAKE_CURRENT_SOURCE_DIR}/librtk.c
    ${CMAKE_CURRENT_SOURCE_DIR}/librtk.h
    )
    
add_library(rtk SHARED ${RTK_SOURCES_C})

target_link_libraries(rtk ${M_LIB} ${RT_LIB} ${CMAKE_THREAD_LIBS_INIT})

if(WIN32)
    target_link_libraries(rtk ws2_32 setupapi cfgmgr32)
else(WIN32)
   set_target_properties(rtk PROPERTIES VERSION ${RTK_VERSION} SOVERSION ${RTK_SOVERSION})
endif(WIN32)


install(TARGETS rtk LIBRARY DESTINATION ${LIB_INSTALL_DIR})
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/FindRTK.cmake DESTINATION "${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_DATADIR}/cmake-${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION}/Modules")

