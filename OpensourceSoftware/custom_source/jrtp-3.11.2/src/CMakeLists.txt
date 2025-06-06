add_definitions(-DJRTPLIB_COMPILING)

set (HEADERS
	rtcpapppacket.h
	rtcpbyepacket.h
	rtcpcompoundpacket.h
	rtcpcompoundpacketbuilder.h
	rtcppacket.h
	rtcppacketbuilder.h
	rtcprrpacket.h
	rtcpscheduler.h
	rtcpsdesinfo.h
	rtcpsdespacket.h
	rtcpsrpacket.h
	rtcpunknownpacket.h
	rtpaddress.h
	rtpcollisionlist.h
	${PROJECT_BINARY_DIR}/src/rtpconfig.h
	rtpdebug.h
	rtpdefines.h
	rtperrors.h
	rtphashtable.h
	rtpinternalsourcedata.h
	rtpipv4address.h
	rtpipv4destination.h
	rtpipv6address.h
	rtpipv6destination.h
	rtpkeyhashtable.h
	rtplibraryversion.h
	rtpmemorymanager.h
	rtpmemoryobject.h
	rtppacket.h
	rtppacketbuilder.h
	rtppollthread.h
	rtprandom.h
	rtprandomrand48.h
	rtprandomrands.h
	rtprandomurandom.h
	rtprawpacket.h
	rtpsession.h
	rtpsessionparams.h
	rtpsessionsources.h
	rtpsourcedata.h
	rtpsources.h
	rtpstructs.h
	rtptimeutilities.h
	rtptransmitter.h
	rtptypes_win.h
	${PROJECT_BINARY_DIR}/src/rtptypes.h
	rtpudpv4transmitter.h
	rtpudpv6transmitter.h  
	rtpbyteaddress.h
	rtpexternaltransmitter.h
	rtpsecuresession.h
	rtpsocketutil.h
	rtpabortdescriptors.h
	rtpselect.h
	rtptcpaddress.h
	rtptcptransmitter.h
	)

set(SOURCES
	rtcpapppacket.cpp
	rtcpbyepacket.cpp
	rtcpcompoundpacket.cpp
	rtcpcompoundpacketbuilder.cpp
	rtcppacket.cpp
	rtcppacketbuilder.cpp
	rtcprrpacket.cpp
	rtcpscheduler.cpp
	rtcpsdesinfo.cpp
	rtcpsdespacket.cpp
	rtcpsrpacket.cpp
	rtpcollisionlist.cpp
	rtpdebug.cpp
	rtperrors.cpp
	rtpinternalsourcedata.cpp
	rtpipv4address.cpp
	rtpipv6address.cpp
	rtpipv4destination.cpp
	rtpipv6destination.cpp
	rtplibraryversion.cpp
	rtppacket.cpp
	rtppacketbuilder.cpp
	rtppollthread.cpp
	rtprandom.cpp
	rtprandomrand48.cpp
	rtprandomrands.cpp
	rtprandomurandom.cpp
	rtpsession.cpp
	rtpsessionparams.cpp
	rtpsessionsources.cpp
	rtpsourcedata.cpp
	rtpsources.cpp
	rtptimeutilities.cpp
	rtpudpv4transmitter.cpp
	rtpudpv6transmitter.cpp 
	rtpbyteaddress.cpp
	rtpexternaltransmitter.cpp
	rtpsecuresession.cpp
	rtpabortdescriptors.cpp
	rtptcpaddress.cpp
	rtptcptransmitter.cpp
	)

if (NOT JRTPLIB_WINSOCK)
	set (SOURCES ${SOURCES} extratransmitters/rtpfaketransmitter.cpp )
	set (HEADERS ${HEADERS} extratransmitters/rtpfaketransmitter.h )
endif ()

if (MSVC)
	set(CMAKE_DEBUG_POSTFIX _d)
endif ()

# Enable extra compiler warnings and mark warnings as errors: -Wall -Wextra -Werror
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_COMPILER_IS_GNUCC)
	option(JRTPLIB_WARNINGSASERRORS "Enable -Wall -Wextra" ON)
	if (JRTPLIB_WARNINGSASERRORS)
		set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra")
	endif ()
endif(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_COMPILER_IS_GNUCC)

if (NOT MSVC OR JRTPLIB_COMPILE_STATIC)
	add_library(jrtplib-static STATIC ${SOURCES} ${HEADERS})
	if (NOT MSVC)
		set_target_properties(jrtplib-static PROPERTIES OUTPUT_NAME jrtp)
	else ()
		set_target_properties(jrtplib-static PROPERTIES OUTPUT_NAME jrtplib)
	endif()
	set_target_properties(jrtplib-static PROPERTIES CLEAN_DIRECT_OUTPUT 1)
	set(JRTPLIB_INSTALLTARGETS jrtplib-static)
	target_link_libraries(jrtplib-static ${JRTPLIB_LINK_LIBS})
endif (NOT MSVC OR JRTPLIB_COMPILE_STATIC)

if (NOT MSVC OR NOT JRTPLIB_COMPILE_STATIC)
	add_library(jrtplib-shared SHARED ${SOURCES} ${HEADERS})
	if (NOT MSVC)
		set_target_properties(jrtplib-shared PROPERTIES OUTPUT_NAME jrtp)
	else ()
		set_target_properties(jrtplib-shared PROPERTIES OUTPUT_NAME jrtplib)
	endif()
	set_target_properties(jrtplib-shared PROPERTIES VERSION ${VERSION})
	set_target_properties(jrtplib-shared PROPERTIES CLEAN_DIRECT_OUTPUT 1)
	set(JRTPLIB_INSTALLTARGETS ${JRTPLIB_INSTALLTARGETS} jrtplib-shared)
	target_link_libraries(jrtplib-shared ${JRTPLIB_LINK_LIBS})
endif (NOT MSVC OR NOT JRTPLIB_COMPILE_STATIC)

apply_include_paths("${JRTPLIB_INTERNAL_INCLUDES}")
apply_include_paths("${JRTPLIB_EXTERNAL_INCLUDES}")

install(FILES ${HEADERS} DESTINATION include/jrtplib3)
install(TARGETS ${JRTPLIB_INSTALLTARGETS} DESTINATION ${LIBRARY_INSTALL_DIR})

