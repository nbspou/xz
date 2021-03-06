
CONFIGURE_FILE("config.h.in" "config.h")

FILE(GLOB LIBLZMA_SRCS_API api/*.h api/lzma/*.h)
FILE(GLOB LIBLZMA_SRCS_CHECK check/*.h check/*.c)
FILE(GLOB LIBLZMA_SRCS_COMMON common/*.h common/*.c)
FILE(GLOB LIBLZMA_SRCS_DELTA delta/*.h delta/*.c)
FILE(GLOB LIBLZMA_SRCS_LZ lz/*.h lz/*.c)
FILE(GLOB LIBLZMA_SRCS_LZMA lzma/*.h lzma/*.c)
FILE(GLOB LIBLZMA_SRCS_RANGECODER rangecoder/*.h rangecoder/*.c)
FILE(GLOB LIBLZMA_SRCS_SIMPLE simple/*.h simple/*.c)
FILE(GLOB LIBLZMA_SRCS_CONFIG ${CMAKE_CURRENT_BINARY_DIR}/config.h)
FILE(GLOB LIBLZMA_SRCS_TUKLIB ../common/*.c ../common/*.h)

LIST(REMOVE_ITEM LIBLZMA_SRCS_CHECK "${CMAKE_CURRENT_SOURCE_DIR}/check/crc32_small.c")
LIST(REMOVE_ITEM LIBLZMA_SRCS_CHECK "${CMAKE_CURRENT_SOURCE_DIR}/check/crc32_tablegen.c")
LIST(REMOVE_ITEM LIBLZMA_SRCS_CHECK "${CMAKE_CURRENT_SOURCE_DIR}/check/crc64_small.c")
LIST(REMOVE_ITEM LIBLZMA_SRCS_CHECK "${CMAKE_CURRENT_SOURCE_DIR}/check/crc64_tablegen.c")
LIST(REMOVE_ITEM LIBLZMA_SRCS_LZMA "${CMAKE_CURRENT_SOURCE_DIR}/lzma/fastpos_tablegen.c")
LIST(REMOVE_ITEM LIBLZMA_SRCS_RANGECODER "${CMAKE_CURRENT_SOURCE_DIR}/rangecoder/price_tablegen.c")

SOURCE_GROUP("api" FILES ${LIBLZMA_SRCS_API})
SOURCE_GROUP("check" FILES ${LIBLZMA_SRCS_CHECK})
SOURCE_GROUP("common" FILES ${LIBLZMA_SRCS_COMMON})
SOURCE_GROUP("delta" FILES ${LIBLZMA_SRCS_DELTA})
SOURCE_GROUP("lz" FILES ${LIBLZMA_SRCS_LZ})
SOURCE_GROUP("lzma" FILES ${LIBLZMA_SRCS_LZMA})
SOURCE_GROUP("rangecoder" FILES ${LIBLZMA_SRCS_RANGECODER})
SOURCE_GROUP("simple" FILES ${LIBLZMA_SRCS_SIMPLE})
SOURCE_GROUP("config" FILES ${LIBLZMA_SRCS_CONFIG})
SOURCE_GROUP("tuklib" FILES ${LIBLZMA_SRCS_TUKLIB})

ADD_LIBRARY(lzma
  ${LIBLZMA_SRCS_API}
  ${LIBLZMA_SRCS_CHECK}
  ${LIBLZMA_SRCS_COMMON}
  ${LIBLZMA_SRCS_DELTA}
  ${LIBLZMA_SRCS_LZ}
  ${LIBLZMA_SRCS_LZMA}
  ${LIBLZMA_SRCS_RANGECODER}
  ${LIBLZMA_SRCS_SIMPLE}
  ${LIBLZMA_SRCS_CONFIG}
  ${LIBLZMA_SRCS_TUKLIB}
)

TARGET_INCLUDE_DIRECTORIES(lzma PRIVATE
  ${CMAKE_CURRENT_BINARY_DIR}
)

IF (BUILD_SHARED_LIBS)
  SET_TARGET_PROPERTIES(lzma PROPERTIES
    COMPILE_DEFINITIONS "HAVE_CONFIG_H;DLL_EXPORT"
  )
ELSE ()
  SET_TARGET_PROPERTIES(lzma PROPERTIES
    COMPILE_DEFINITIONS "HAVE_CONFIG_H"
  )
ENDIF ()
