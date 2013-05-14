include(${SOFA_CMAKE_DIR}/preProject.cmake)

##plugin external dependencies
#find_package( REQUIRED)
#include_directories(${})

set(HEADER_FILES

    initOptiTrackNatNet.h 
	OptiTrackNatNetClient.h 
	OptiTrackNatNetDevice.h

	)

set(SOURCE_FILES

    initOptiTrackNatNet.cpp 
	OptiTrackNatNetClient.cpp 
	OptiTrackNatNetDevice.cpp

	)

add_library(${PROJECT_NAME} SHARED ${HEADER_FILES} ${SOURCE_FILES})

set(COMPILER_DEFINES "SOFA_BUILD_OPTITRACKNATNET")
set(LINKER_DEPENDENCIES SofaCore SofaSimulationCommon SofaUserInteraction boost )

include(${SOFA_CMAKE_DIR}/postProject.cmake)