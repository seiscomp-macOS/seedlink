FIND_PACKAGE(OpenSSL REQUIRED)

SET(SLARCHIVE_SOURCES
	archive.c
	dsarchive.c
	slarchive.c
)

INCLUDE_DIRECTORIES(../../../../libs/3rd-party/libslink)

ADD_EXECUTABLE(slarchive ${SLARCHIVE_SOURCES})

IF(APPLE)
    TARGET_LINK_LIBRARIES(slarchive slink ${crypto})
    INCLUDE_DIRECTORIES(${OPENSSL_INCLUDE_DIR})
ELSE()
    TARGET_LINK_LIBRARIES(slarchive slink crypto)
ENDIF(APPLE)

INSTALL(TARGETS slarchive
	RUNTIME DESTINATION ${SC3_PACKAGE_BIN_DIR})
