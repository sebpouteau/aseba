set (ASEBACOMMON_SRC
	utils/FormatableString.cpp
	utils/utils.cpp
	utils/HexFile.cpp
	utils/BootloaderInterface.cpp
	msg/msg.cpp
	msg/NodesManager.cpp
)

find_package(DNSSD)
if(DNSSD_FOUND)
	include_directories(${DNSSD_INCLUDE_DIRS})
	set(ASEBACOMMON_SRC
		${ASEBACOMMON_SRC}
		zeroconf/zeroconf.cpp
		zeroconf/txtrecord.cpp
		zeroconf/target.cpp
	)
endif(DNSSD_FOUND)

add_library(asebacommon ${ASEBACOMMON_SRC})
set_target_properties(asebacommon PROPERTIES VERSION ${LIB_VERSION_STRING} 
                                        SOVERSION ${LIB_VERSION_MAJOR})
if(DNSSD_FOUND)
	target_include_directories(asebacommon PUBLIC ${DNSSD_INCLUDE_DIRS})
endif(DNSSD_FOUND)

install(TARGETS asebacommon 
		LIBRARY DESTINATION ${LIB_INSTALL_DIR} 
		ARCHIVE DESTINATION ${LIB_INSTALL_DIR} 
) 

set (ASEBACORE_HDR_UTILS 
	utils/utils.h
	utils/FormatableString.h
)
set (ASEBACORE_HDR_MSG
	msg/msg.h
	msg/NodesManager.h
)
set (ASEBACORE_HDR_COMMON
	consts.h
	types.h
	productids.h
)
set (ASEBACORE_HDR_ZEROCONF
	zeroconf/zeroconf.h
)
install(FILES ${ASEBACORE_HDR_UTILS}
	DESTINATION include/aseba/common/utils
)
install(FILES ${ASEBACORE_HDR_MSG}
	DESTINATION include/aseba/common/msg
)
install(FILES ${ASEBACORE_HDR_COMMON}
	DESTINATION include/aseba/common
)
if(DNSSD_FOUND)
	install(FILES ${ASEBACORE_HDR_ZEROCONF}
		DESTINATION include/aseba/common/msg
	)
endif(DNSSD_FOUND)
