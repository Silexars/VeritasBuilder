IF(COMMAND VERITAS_SET_PROPERTIES) # Cheap include guard
RETURN()
ENDIF()

INCLUDE("Compiler.cmake")
INCLUDE("Architecture.cmake")
INCLUDE("Platform.cmake")

FUNCTION(VERITAS_SET_PROPERTIES TARGET)
    VERITAS_SET_COMPILER_PROPERTIES(${TARGET} ${ARGN})
    VERITAS_SET_ARCHITECTURE_PROPERTIES(${TARGET} ${ARGN})
    VERITAS_SET_PLATFORM_PROPERTIES(${TARGET} ${ARGN})
ENDFUNCTION()

FUNCTION(ADD_EXECUTABLE TARGET PARAMETER)
    IF (NOT (PARAMETER STREQUAL "WALLPAPER" OR PARAMETER STREQUAL "SERVICE"))
        LIST(INSERT ARGN 0 ${PARAMETER})
    ENDIF()

    IF (${CMAKE_SYSTEM_NAME} STREQUAL "Android")
        ADD_LIBRARY(${TARGET} MODULE ${ARGN})
    ELSE()
        _ADD_EXECUTABLE(${TARGET} ${ARGN})
    ENDIF()
    VERITAS_SET_PROPERTIES(${ARGV})
    VERITAS_PLATFORM_ADD_EXECUTABLE(${ARGV})
ENDFUNCTION()

FUNCTION(ADD_LIBRARY TARGET)
    _ADD_LIBRARY(${TARGET} ${ARGN})
    VERITAS_SET_PROPERTIES(${TARGET} ${ARGN})
ENDFUNCTION()
