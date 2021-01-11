# ================================================================= Project properties ===

# Minimum required version of CMake to configure the project
cmake_minimum_required (VERSION 3.12)

# Name of the project handled by CMake
project(ntlm
        DESCRIPTION "An implementation of ntlm negotiation message using C++"
       )

# ============================================================== Configuration options ===

option (OPENSSL_USE_STATIC_LIBS "OpenSSL use static libraries"  ON)

# ================================================================== System inspection ===

include (FindPackageHandleStandardArgs)
include (CheckTypeSize)

find_package(OpenSSL REQUIRED)

if (CMAKE_SIZEOF_VOID_P EQUAL 8)
    set (CMAKE_SYSTEM_64BIT NO)
else (CMAKE_SIZEOF_VOID_P EQUAL 8)
    set (CMAKE_SYSTEM_64BIT YES)
endif (CMAKE_SIZEOF_VOID_P EQUAL 8)

# ================================================================= Build instructions ===

include_directories (${PROJECT_SOURCE_DIR}/include)

add_library(${PROJECT_NAME}
    src/ntlm.cpp
    src/util.cpp
    src/libcrypto-compat.c
    # header files
    include/ntlm/ntlm.h
    src/util.h
)

target_compile_options(${PROJECT_NAME}
    PRIVATE
        -Werror
)

target_include_directories(${PROJECT_NAME} BEFORE
    PUBLIC
        "${PROJECT_SOURCE_DIR}/include"

    PRIVATE
        "${OPENSSL_INCLUDE_DIR}"
)

target_link_libraries(${PROJECT_NAME}
    PUBLIC
        "${OPENSSL_LIBRARIES}"
)

# ========================================================================================

message ("--------------------------------------------------------------------------------" )
message (" [${PROJECT_NAME}] Configuration summary"                                         )
message ("--------------------------------------------------------------------------------" )
# ========================================================================================
message ("")
message (" >> System configuration"                                                         )
message ("    -- Processor type ........................... = ${CMAKE_SYSTEM_PROCESSOR}"    )
message ("    -- System name & version .................... = ${CMAKE_SYSTEM_VERSION}"      )
message ("    -- Operating system name .................... = ${CMAKE_SYSTEM_NAME}"         )
message ("    -- System is 64-bit? ........................ = ${CMAKE_SYSTEM_64BIT}"        )
message ("    -- User name ................................ = ${CMAKE_SYSTEM_USERNAME}"     )
message ("    -- User home directory ...................... = ${CMAKE_SYSTEM_USERHOME}"     )
message ("    -- System PATH variable ..................... = ${CMAKE_SYSTEM_PATH}"         )
# ========================================================================================
message ("")
message (" >> Project configuration"                                                        )
message ("    -- Project name ............................. = ${PROJECT_NAME}"              )
message ("    -- Project description ...................... = ${PROJECT_DESCRIPTION}"       )
message ("    -- Project version .......................... = ${PROJECT_VERSION}"           )
message ("")
message ("--------------------------------------------------------------------------------" )
