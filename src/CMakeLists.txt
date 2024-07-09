cmake_minimum_required(VERSION 3.16)

find_package(Boost REQUIRED CONFIG COMPONENTS thread interprocess)
find_package(ZLIB REQUIRED)
find_package(lz4 CONFIG REQUIRED)
find_package(mo2-dds-header CONFIG REQUIRED)

add_library(bsatk STATIC)
target_link_libraries(bsatk
	PUBLIC mo2::dds-header
	PRIVATE ZLIB::ZLIB lz4::lz4 Boost::thread Boost::interprocess
)

target_sources(bsatk
	PRIVATE
		bsaarchive.cpp
		bsaexception.cpp
		bsafile.cpp
		bsafolder.cpp
		bsatypes.cpp
		filehash.cpp
	PUBLIC
		FILE_SET HEADERS
		BASE_DIRS ${CMAKE_CURRENT_LIST_DIR}/../include
		FILES
		${CMAKE_CURRENT_LIST_DIR}/../include/bsatk/bsaarchive.h
		${CMAKE_CURRENT_LIST_DIR}/../include/bsatk/bsaexception.h
		${CMAKE_CURRENT_LIST_DIR}/../include/bsatk/bsafile.h
		${CMAKE_CURRENT_LIST_DIR}/../include/bsatk/bsafolder.h
		${CMAKE_CURRENT_LIST_DIR}/../include/bsatk/bsatk.h
		${CMAKE_CURRENT_LIST_DIR}/../include/bsatk/bsatypes.h
		${CMAKE_CURRENT_LIST_DIR}/../include/bsatk/errorcodes.h
		${CMAKE_CURRENT_LIST_DIR}/../include/bsatk/filehash.h

)

set_target_properties(bsatk PROPERTIES CXX_STANDARD 20)

# for building
target_include_directories(bsatk PRIVATE ${CMAKE_CURRENT_LIST_DIR}/../include/bsatk)

if (MSVC)
	target_compile_options(bsatk
		PRIVATE
		"/MP"
		"/Wall"
		"/external:anglebrackets"
		"/external:W0"
	)
	target_link_options(bsatk
		PRIVATE
		$<$<CONFIG:RelWithDebInfo>:/LTCG /INCREMENTAL:NO /OPT:REF /OPT:ICF>
	)

	set_target_properties(bsatk PROPERTIES VS_STARTUP_PROJECT bsatk)
endif()

add_library(mo2::bsatk ALIAS bsatk)

# isntall
install(TARGETS bsatk EXPORT bsatkTargets FILE_SET HEADERS)
install(EXPORT bsatkTargets
	FILE mo2-bsatk-targets.cmake
	NAMESPACE mo2::
	DESTINATION lib/cmake/mo2-bsatk
)
