cmake_minimum_required(VERSION 2.8.3)
project(rtklib)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -Wall -O3 -ansi -pedantic -Wno-unused-but-set-variable")

find_package(catkin REQUIRED)
find_package(Threads REQUIRED)

include_directories(include/rtklib ${catkin_INCLUDE_DIRS})

set(HEADER_FILES src/rtklib.h)

catkin_package(
		INCLUDE_DIRS include
		LIBRARIES ${PROJECT_NAME})

include_directories(include ${catkin_INCLUDE_DIRS})

set(CMAKE_CXX_FLAGS "-DDLL ${CMAKE_CXX_FLAGS}")

add_library(${PROJECT_NAME} src/convgpx.c src/convkml.c src/convrnx.c src/datum.c src/download.c src/ephemeris.c
		src/geoid.c src/gis.c src/ionex.c src/lambda.c src/options.c src/pntpos.c src/postpos.c src/ppp.c src/ppp_ar.c
		src/ppp_corr.c src/preceph.c src/qzslex.c src/rcvraw.c src/rinex.c src/rtcm.c src/rtcm2.c src/rtcm3.c src/rtcm3e.c
		src/rtkcmn.c src/rtkpos.c src/rtksvr.c src/sbas.c src/solution.c src/stream.c src/streamsvr.c src/tides.c
		src/tle.c src/rcv/cmr.c src/rcv/binex.c src/rcv/crescent.c src/rcv/gw10.c src/rcv/javad.c src/rcv/novatel.c
		src/rcv/nvs.c src/rcv/rcvlex.c src/rcv/rt17.c src/rcv/septentrio.c src/rcv/skytraq.c src/rcv/ss2.c
		src/rcv/ublox.c)

add_executable(rtkrcv app/rtkrcv/rtkrcv.c app/rtkrcv/vt.c ${HEADER_FILES})
target_link_libraries(rtkrcv
		${PROJECT_NAME}
		${CMAKE_THREAD_LIBS_INIT}
		m
		rt)
set_target_properties(rtkrcv PROPERTIES
		COMPILE_FLAGS "-DENAGLO -DENAQZS -DENACMP -DENAGAL -DENAIRN -DNFREQ=3 -DSVR_REUSEADDR")

add_executable(str2str app/str2str/str2str.c ${HEADER_FILES})
target_link_libraries(str2str
		${PROJECT_NAME}
		${CMAKE_THREAD_LIBS_INIT}
		m
		rt)
set_target_properties(str2str PROPERTIES
		COMPILE_FLAGS "-DENAGLO -DENAGAL -DENAQZS -DENACMP -DENAIRN -DTRACE -DNFREQ=3 -DNEXOBS=3 -DSVR_REUSEADDR")

install(DIRECTORY include/${PROJECT_NAME}/
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})

install(TARGETS ${PROJECT_NAME}
#  PUBlIC_HEADER DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
  )
