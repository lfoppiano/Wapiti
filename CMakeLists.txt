cmake_minimum_required(VERSION 3.5)
project( wapiti )

set(CMAKE_BUILD_TYPE "Release")

set(CMAKE_C_FLAGS "-std=c99")

set(CMAKE_SKIP_BUILD_RPATH FALSE)
set(CMAKE_BUILD_WITH_INSTALL_RPATH FALSE)

set(CMAKE_EXE_LINKER_FLAGS
    "${CMAKE_EXE_LINKER_FLAGS} -pthread -Wl,-rpath,'$ORIGIN/'" )

set(CMAKE_SHARED_LINKER_FLAGS
    "${CMAKE_SHARED_LINKER_FLAGS} -pthread -Wl,-rpath,'$ORIGIN/'" )

find_package(JNI REQUIRED)

if (JNI_FOUND)
    include_directories( ${JNI_INCLUDE_DIRS} )
endif (JNI_FOUND)

AUX_SOURCE_DIRECTORY(src SOURCES)

AUX_SOURCE_DIRECTORY(src/swig SOURCES_SWIG)
if(APPLE)
elseif(UNIX)
  list(REMOVE_ITEM SOURCES_SWIG src/swig/fmemopen.c)
endif()
message ("Swig sources list: ${SOURCES_SWIG}")

#SET_SOURCE_FILES_PROPERTIES( ${SOURCES} PROPERTIES LANGUAGE CXX )

add_library( wapiti SHARED ${SOURCES} ${SOURCES_SWIG})

add_executable( wapiti_runner ${SOURCES} ${SOURCES_SWIG})

set( CMAKE_VERBOSE_MAKEFILE on )
