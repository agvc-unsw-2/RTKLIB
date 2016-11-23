cmake_minimum_required(VERSION 2.8.3)
project(rtklib)

find_package(catkin REQUIRED)

include_directories(include ${catkin_INCLUDE_DIRS})

catkin_package(
		INCLUDE_DIRS include
		LIBRARIES ${PROJECT_NAME})

include_directories(include ${catkin_INCLUDE_DIRS})

set(CMAKE_CXX_FLAGS "-DDLL ${CMAKE_CXX_FLAGS}")

add_library(${PROJECT_NAME} src/convkml.c src/convrnx.c src/datum.c src/download.c src/ephemeris.c
		src/geoid.c src/ionex.c src/lambda.c src/options.c src/pntpos.c src/postpos.c src/ppp.c src/ppp_ar.c
		src/preceph.c src/qzslex.c src/rcvraw.c src/rinex.c src/rtcm.c src/rtcm2.c src/rtcm3.c src/rtcm3e.c
		src/rtkcmn.c src/rtkpos.c src/rtksvr.c src/sbas.c src/solution.c src/stream.c src/streamsvr.c
		src/tle.c src/rcv/binex.c src/rcv/crescent.c src/rcv/gw10.c src/rcv/javad.c src/rcv/novatel.c
		src/rcv/nvs.c src/rcv/rcvlex.c src/rcv/rt17.c src/rcv/septentrio.c src/rcv/skytraq.c src/rcv/ss2.c
		src/rcv/ublox.c)

install(DIRECTORY include/${PROJECT_NAME}/
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})

install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
  )
