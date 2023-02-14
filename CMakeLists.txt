cmake_minimum_required(VERSION 3.12)
project(OptiTrackNatNet)

set(OPTITRACKNATNET_SRC "src/${PROJECT_NAME}")

set(HEADER_FILES
    ${OPTITRACKNATNET_SRC}/OptiTrackNatNetClient.h
    ${OPTITRACKNATNET_SRC}/OptiTrackNatNetDevice.h
    ${OPTITRACKNATNET_SRC}/config.h.in
)

set(SOURCE_FILES
    ${OPTITRACKNATNET_SRC}/OptiTrackNatNetClient.cpp
    ${OPTITRACKNATNET_SRC}/OptiTrackNatNetDevice.cpp
    ${OPTITRACKNATNET_SRC}/initOptiTrackNatNet.cpp
)

sofa_find_package(Sofa.Core REQUIRED)
sofa_find_package(Sofa.Simulation.Common REQUIRED)
sofa_find_package(Sofa.Component.Controller REQUIRED)
sofa_find_package(Sofa.GL REQUIRED)
sofa_find_package(Boost QUIET COMPONENTS system regex date_time REQUIRED)

add_library(${PROJECT_NAME} SHARED ${HEADER_FILES} ${SOURCE_FILES})

if(MSVC)
    #define BOOST_ALL_DYN_LINK needed for dynamic linking with boost libraries
    target_compile_definitions(${PROJECT_NAME} PUBLIC BOOST_ALL_DYN_LINK)
endif()

target_link_libraries(${PROJECT_NAME} Sofa.Core Sofa.Simulation.Common Sofa.GL Sofa.Component.Controller)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES})

# Seems to be required by boost::asio
if(UNIX)
    target_link_libraries(${PROJECT_NAME} pthread)
endif()

sofa_create_package_with_targets(
    PACKAGE_NAME ${PROJECT_NAME}
    PACKAGE_VERSION ${Sofa_VERSION}
    TARGETS ${PROJECT_NAME} AUTO_SET_TARGET_PROPERTIES
    INCLUDE_SOURCE_DIR "src"
    INCLUDE_INSTALL_DIR "${PROJECT_NAME}"
    RELOCATABLE "plugins"
)
