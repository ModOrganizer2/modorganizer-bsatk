CMAKE_MINIMUM_REQUIRED (VERSION 2.8)

SET(PROJ_AUTHOR Tannin)
SET(PROJ_ARCH x86)
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
  SET(PROJ_ARCH x64)
endif()


SET(bsatk_SRCS
    filehash.cpp
    bsafile.cpp
    bsaexception.cpp
    bsafolder.cpp
    bsaarchive.cpp
    bsatypes.cpp
  )

SET(bsatk_HDRS
    filehash.h
    errorcodes.h
    bsatypes.h
    bsafile.h
    bsafolder.h
    bsaexception.h
    bsaarchive.h
    dxgiformat.h
    DDS.h
  )

SET(Boost_USE_STATIC_LIBS        ON)
SET(Boost_USE_MULTITHREADED      ON)
SET(Boost_USE_STATIC_RUNTIME    OFF)
FIND_PACKAGE(Boost REQUIRED)

FIND_PACKAGE(zlib REQUIRED)

INCLUDE_DIRECTORIES(${Boost_INCLUDE_DIRS}
                    ${ZLIB_INCLUDE_DIRS}
                    ${ZLIB_INCLUDE_DIRS}/build # in case of an out-of-source build
                    ${LZ4_ROOT}/include)

LINK_DIRECTORIES(${LZ4_ROOT}/dll)

ADD_LIBRARY(bsatk STATIC ${bsatk_HDRS} ${bsatk_SRCS})

TARGET_LINK_LIBRARIES(liblz4)

IF (NOT "${OPTIMIZE_COMPILE_FLAGS}" STREQUAL "")
  SET_TARGET_PROPERTIES(bsatk PROPERTIES COMPILE_FLAGS_RELWITHDEBINFO
                        ${OPTIMIZE_COMPILE_FLAGS})
ENDIF()
SET_TARGET_PROPERTIES(bsatk PROPERTIES LINK_FLAGS_RELWITHDEBINFO
                      "/LARGEADDRESSAWARE ${OPTIMIZE_LINK_FLAGS}")

###############
## Installation

INSTALL(TARGETS bsatk
        ARCHIVE DESTINATION libs)
