cmake_minimum_required (VERSION 3.0)

project (lightclient C)

if(DTLS)
    message(FATAL_ERROR "DTLS option is not supported." )
endif()

include(${CMAKE_CURRENT_LIST_DIR}/../../core/wakaama.cmake)
include(${CMAKE_CURRENT_LIST_DIR}/../shared/shared.cmake)

add_definitions(-DLWM2M_CLIENT_MODE)
if(LWM2M_VERSION VERSION_GREATER "1.0")
    add_definitions(-DLWM2M_SUPPORT_SENML_JSON)
endif()

add_definitions(${SHARED_DEFINITIONS} ${WAKAAMA_DEFINITIONS})

include_directories (${WAKAAMA_SOURCES_DIR} ${SHARED_INCLUDE_DIRS})

SET(SOURCES
    ${CMAKE_CURRENT_LIST_DIR}/lightclient.c
    ${CMAKE_CURRENT_LIST_DIR}/object_security.c
    ${CMAKE_CURRENT_LIST_DIR}/object_server.c
    ${CMAKE_CURRENT_LIST_DIR}/object_device.c
    ${CMAKE_CURRENT_LIST_DIR}/object_test.c
    )

add_executable(${PROJECT_NAME} ${SOURCES} ${WAKAAMA_SOURCES} ${SHARED_SOURCES})

# Add WITH_LOGS to debug variant
set_property(TARGET ${PROJECT_NAME} APPEND PROPERTY COMPILE_DEFINITIONS $<$<CONFIG:Debug>:WITH_LOGS>)

SOURCE_GROUP(wakaama FILES ${WAKAAMA_SOURCES})
