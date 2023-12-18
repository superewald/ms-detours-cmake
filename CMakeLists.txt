cmake_minimum_required(VERSION 3.8)

project(MsDetours)

option(MS_DETOURS_INSTALL "Install MsDetours" OFF)

# Detours
set(TARGET MsDetours)
set(DETOURS_SOURCE Detours/src)

add_library(${TARGET} STATIC
    ${DETOURS_SOURCE}/creatwth.cpp
    ${DETOURS_SOURCE}/detours.cpp
    ${DETOURS_SOURCE}/detours.h
    ${DETOURS_SOURCE}/detver.h
    ${DETOURS_SOURCE}/disasm.cpp
    ${DETOURS_SOURCE}/disolarm.cpp
    ${DETOURS_SOURCE}/disolarm64.cpp
    ${DETOURS_SOURCE}/disolia64.cpp
    ${DETOURS_SOURCE}/disolx64.cpp
    ${DETOURS_SOURCE}/disolx86.cpp
    ${DETOURS_SOURCE}/image.cpp
    ${DETOURS_SOURCE}/modules.cpp
    ${DETOURS_SOURCE}/uimports.cpp)

# This file is included and not compiled on its own
set_property (
    SOURCE ${DETOURS_SOURCE}/uimports.cpp
    APPEND PROPERTY HEADER_FILE_ONLY true)

target_compile_options(${TARGET} PRIVATE /W4 /WX /Zi /MT /Gy /Gm- /Zl /Od)
target_include_directories(${TARGET} PUBLIC ${DETOURS_SOURCE})

include(cmake/cpm.cmake)
CPMAddPackage("gh:TheLartians/PackageProject.cmake@1.11.1")

if(MS_DETOURS_INSTALL)
    packageProject(
        NAME ${PROJECT_NAME}
        VERSION ${PROJECT_VERSION}
        NAMESPACE ${PROJECT_NAME}
        BINARY_DIR ${PROJECT_BINARY_DIR}
        INCLUDE_DIR ${PROJECT_SOURCE_DIR/Detours/src}
        INCLUDE_DESTINATION include/${PROJECT_NAME}
        INCLUDE_HEADER_PATTERN "*.h"
    )
endif()