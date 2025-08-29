SET(LIBCAPS_SOURCES
	libs/gempa/caps/application.cpp
	libs/gempa/caps/connection.cpp
	libs/gempa/caps/datetime.cpp
	libs/gempa/caps/log.cpp
	libs/gempa/caps/mseedpacket.cpp
	libs/gempa/caps/packet.cpp
	libs/gempa/caps/rawpacket.cpp
	libs/gempa/caps/riff.cpp
	libs/gempa/caps/sessiontable.cpp
	libs/gempa/caps/socket.cpp
	libs/gempa/caps/strptime.c
)

SET(CAPSPLUGIN_SOURCES
	app.cpp
	main.cpp
	${LIBCAPS_SOURCES}
)

INCLUDE_DIRECTORIES(.)
INCLUDE_DIRECTORIES(../../libs/plugin)
INCLUDE_DIRECTORIES(../../libs/3rd-party/libslink)
INCLUDE_DIRECTORIES(libs)
INCLUDE_DIRECTORIES(${Boost_INCLUDE_DIRS})
INCLUDE_DIRECTORIES(${SC_SYSTEM_LIB_DIR}/3rd-party/libmseed)

ADD_EXECUTABLE(caps_plugin ${CAPSPLUGIN_SOURCES})

FIND_PACKAGE(OpenSSL REQUIRED)

# Since Boost 1.70.0, the system library is considered a header-only library when using C++11 or later,
# so skip searching for "Boost COMPONENTS system" (will leave Boost_system_LIBRARY to empty)
# to avoid error with Boost 1.89+ => Boost_System_NOTFOUND
IF(APPLE)
    INCLUDE_DIRECTORIES(${OPENSSL_INCLUDE_DIR})
    FIND_PACKAGE(Boost REQUIRED COMPONENTS program_options)
ELSE()
    FIND_PACKAGE(Boost REQUIRED COMPONENTS system program_options)
ENDIF(APPLE)

TARGET_LINK_LIBRARIES(
	caps_plugin
		slink
		slplugin
		slutils
		mseed
		${Boost_system_LIBRARY}
		${Boost_program_options_LIBRARY}
		${CRYPTO}
		${SSL}
)

INSTALL(TARGETS caps_plugin RUNTIME DESTINATION ${SEEDLINK_PLUGIN_OUTPUT_DIR})

FILE(GLOB descs "${CMAKE_CURRENT_SOURCE_DIR}/descriptions/*.xml")
INSTALL(FILES ${descs} DESTINATION ${SC3_PACKAGE_APP_DESC_DIR})
