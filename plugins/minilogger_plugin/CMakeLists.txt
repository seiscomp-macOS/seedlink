SET(MINILOGGER_PLUGIN_SOURCES
	mini_logger_writer.c
	mini_logger_writer.h
	minisepdevice.c
	minisepdevice.h
	settings/settings.c
	settings/settings.h
	settings/strmap.c
	settings/strmap.h
)

INCLUDE_DIRECTORIES(../../libs/plugin ${LIBMSEED_INCLUDE_DIR})
ADD_EXECUTABLE(minilogger_plugin ${MINILOGGER_PLUGIN_SOURCES})
IF (APPLE)
    # On macOS don't use librt (realtime lib)
    TARGET_LINK_LIBRARIES(minilogger_plugin slplugin ${LIBMSEED_LIBRARY})
ELSE()
    TARGET_LINK_LIBRARIES(minilogger_plugin rt slplugin ${LIBMSEED_LIBRARY})
ENDIF (APPLE)
INSTALL(TARGETS minilogger_plugin RUNTIME DESTINATION ${SEEDLINK_PLUGIN_OUTPUT_DIR})

