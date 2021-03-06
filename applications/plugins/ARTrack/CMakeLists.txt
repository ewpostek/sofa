cmake_minimum_required(VERSION 3.1)
project(ARTrack)


set(HEADER_FILES
    ARTrackController.h
    ARTrackController.inl
    ARTrackDriver.h
    ARTrackEvent.h
)

set(SOURCE_FILES
    ARTrackController.cpp
    ARTrackDriver.cpp
    ARTrackEvent.cpp
    initARTrack.cpp
)

find_package(ARTrackLib REQUIRED)
find_package(SofaGeneral REQUIRED)

add_library(${PROJECT_NAME} SHARED ${HEADER_FILES} ${SOURCE_FILES})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_FLAGS "-DSOFA_BUILD_ARTRACKPLUGIN")
target_link_libraries(${PROJECT_NAME} SofaUserInteraction SofaGeneralRigid)
target_link_libraries(${PROJECT_NAME} ARTrackLib)
include_directories("${CMAKE_CURRENT_SOURCE_DIR}/..")

if(WIN32)
    target_link_libraries(${PROJECT_NAME} Ws2_32)
endif()

if(UNIX)
    target_link_libraries(${PROJECT_NAME} pthread)
endif()

install(TARGETS ${PROJECT_NAME}
        COMPONENT ARTrack_libraries
        EXPORT ARTrackTargets
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)
