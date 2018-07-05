########################################################################
# CMake project
PROJECT("XZ Utils")

# Minimum required 3.1 for c++ standard macro
CMAKE_MINIMUM_REQUIRED(VERSION 3.1 FATAL_ERROR)
# SET(CMAKE_VERBOSE_MAKEFILE ON)

IF (POLICY CMP0020)
	CMAKE_POLICY(SET CMP0020 NEW)
ENDIF ()

INCLUDE(CheckIncludeFiles)
INCLUDE(CheckTypeSize)
SET(CMAKE_CXX_STANDARD 14)


########################################################################
# Ensure that we are not building in our source directories.

SET(Build_Dir_OK "TRUE")
STRING(REGEX MATCH "^${CMAKE_SOURCE_DIR}" In_Sub_Dir ${CMAKE_BINARY_DIR})
IF (In_Sub_Dir)
  STRING(REGEX MATCH "^${CMAKE_SOURCE_DIR}/build" In_Build_Dir ${CMAKE_BINARY_DIR})
  IF (NOT In_Build_Dir)
    SET(Build_Dir_OK "FALSE")
  ENDIF ()
ENDIF ()

IF (NOT Build_Dir_OK)
  MESSAGE(FATAL_ERROR "You must run cmake from a directory that is not in your source tree, or that is in a special subdirectory of the tree whose name begins with 'build'.")
ENDIF ()


########################################################################
# Set up the basic build environment

IF (CMAKE_BUILD_TYPE STREQUAL "")
  # CMake defaults to leaving CMAKE_BUILD_TYPE empty. This messes up
  # differentiation between debug and release builds.
  SET(CMAKE_BUILD_TYPE "RelWithDebInfo" CACHE STRING "Choose the type of build, options are: None Debug Release RelWithDebInfo MinSizeRel." FORCE)
ENDIF ()

SET(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

########################################################################
# Strip binary in release mode

IF (CMAKE_COMPILER_IS_GNUCXX AND !MINGW)
  SET(CMAKE_EXE_LINKER_FLAGS_RELEASE "-s")
ENDIF ()

########################################################################

OPTION(BUILD_SHARED_LIBS "Build shared library" ON)

########################################################################

LINK_DIRECTORIES(
)

INCLUDE_DIRECTORIES(
  ${CMAKE_SOURCE_DIR}/src/liblzma/common
  ${CMAKE_SOURCE_DIR}/src/common
  ${CMAKE_SOURCE_DIR}/src/liblzma/api
  ${CMAKE_SOURCE_DIR}/src/liblzma/check
  ${CMAKE_SOURCE_DIR}/src/liblzma/delta
  ${CMAKE_SOURCE_DIR}/src/liblzma/lz
  ${CMAKE_SOURCE_DIR}/src/liblzma/lzma
  ${CMAKE_SOURCE_DIR}/src/liblzma/rangecoder
  ${CMAKE_SOURCE_DIR}/src/liblzma/simple
)

########################################################################

SET(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
SET(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

########################################################################

CHECK_INCLUDE_FILE("inttypes.h" HAVE_INTTYPES_H)
CHECK_INCLUDE_FILE("limits.h" HAVE_LIMITS_H)
CHECK_INCLUDE_FILE("stdbool.h" HAVE_STDBOOL_H)
CHECK_INCLUDE_FILE("stdint.h" HAVE_STDINT_H)
CHECK_INCLUDE_FILE("stdlib.h" HAVE_STDLIB_H)
CHECK_INCLUDE_FILE("string.h" HAVE_STRING_H)
CHECK_INCLUDE_FILE("unistd.h" HAVE_UNISTD_H)
CHECK_TYPE_SIZE("_Bool" _BOOL)

########################################################################

ADD_SUBDIRECTORY(src/liblzma)
ADD_SUBDIRECTORY(src/xz)

########################################################################
