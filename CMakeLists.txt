cmake_minimum_required(VERSION 3.12)

project(krkrsdl2_env)

if (${CMAKE_VERSION} VERSION_GREATER_EQUAL "3.13")
cmake_policy(SET CMP0077 NEW)
endif()
option(BUILTIN_PLUGIN_ENABLE_KAGPARSER "Enable the KAGParser plugin." ON)
option(BUILTIN_PLUGIN_ENABLE_EXTRANS "Enable the extrans plugin." ON)
option(BUILTIN_PLUGIN_ENABLE_WUTCWF "Enable the wutcwf plugin." ON)
option(BUILTIN_PLUGIN_ENABLE_WUVORBIS "Enable the wuvorbis plugin." ON)
option(BUILTIN_PLUGIN_ENABLE_FSTAT "Enable the fstat plugin." ON)
option(BUILTIN_PLUGIN_ENABLE_JSON "Enable the json plugin." ON)
option(BUILTIN_PLUGIN_ENABLE_VARFILE "Enable the varfile plugin." ON)
option(BUILTIN_PLUGIN_ENABLE_KRGLHWEBP "Enable the krglhwebp plugin." ON)

# Options can be passed to toe build system Kirikiri SDL2 using this method.
set(OPTION_ENABLE_EXTERNAL_PLUGINS OFF)

include(FetchContent)

FetchContent_Declare(
	krkrsdl2
	GIT_REPOSITORY https://github.com/krkrsdl2/krkrsdl2.git
	GIT_TAG        origin/main
)

if(NOT DEFINED KRKRSDL2_REPO_DIR)
	FetchContent_GetProperties(krkrsdl2)
	if(NOT krkrsdl2_POPULATED)
		FetchContent_Populate(krkrsdl2)
		set(KRKRSDL2_REPO_DIR ${krkrsdl2_SOURCE_DIR} CACHE STRING "")
	endif()
endif()
add_subdirectory(${KRKRSDL2_REPO_DIR} ${CMAKE_CURRENT_BINARY_DIR}/krkrsdl2_repo)

# Plugins can be statically linked to Kirikiri SDL2 using this method.
FetchContent_Declare(
	ncbind
	GIT_REPOSITORY https://github.com/krkrsdl2/ncbind.git
	GIT_TAG        origin/krkrsdl2
)

if(NOT DEFINED NCBIND_REPO_DIR)
	FetchContent_GetProperties(ncbind)
	if(NOT ncbind_POPULATED)
		FetchContent_Populate(ncbind)
		set(NCBIND_REPO_DIR ${ncbind_SOURCE_DIR} CACHE STRING "")
	endif()
endif()

set(NCBIND_SRC_DIR ${NCBIND_REPO_DIR})
file(STRINGS ${NCBIND_SRC_DIR}/src_list/sources.txt NCBIND_SRC)
file(STRINGS ${NCBIND_SRC_DIR}/src_list/includes.txt NCBIND_INCLUDES)
list(TRANSFORM NCBIND_SRC PREPEND ${NCBIND_SRC_DIR}/)
list(TRANSFORM NCBIND_INCLUDES PREPEND ${NCBIND_SRC_DIR}/)
list(APPEND KRKRSDL2_EXTRA_SRC ${NCBIND_SRC})
list(APPEND KRKRSDL2_EXTRA_INCLUDES ${NCBIND_INCLUDES})

if(${BUILTIN_PLUGIN_ENABLE_KAGPARSER})
FetchContent_Declare(
	kagparser
	GIT_REPOSITORY https://github.com/krkrsdl2/KAGParser.git
	GIT_TAG        origin/krkrsdl2
)

if(NOT DEFINED KAGPARSER_REPO_DIR)
	FetchContent_GetProperties(kagparser)
	if(NOT kagparser_POPULATED)
		FetchContent_Populate(kagparser)
		set(KAGPARSER_REPO_DIR ${kagparser_SOURCE_DIR} CACHE STRING "")
	endif()
endif()

set(KAGPARSER_SRC_DIR ${KAGPARSER_REPO_DIR})
file(STRINGS ${KAGPARSER_SRC_DIR}/src_list/sources.txt KAGPARSER_SRC)
list(TRANSFORM KAGPARSER_SRC PREPEND ${KAGPARSER_SRC_DIR}/)
list(APPEND KRKRSDL2_EXTRA_SRC ${KAGPARSER_SRC})
endif()
if(${BUILTIN_PLUGIN_ENABLE_EXTRANS})
FetchContent_Declare(
	extrans
	GIT_REPOSITORY https://github.com/krkrsdl2/SamplePlugin.git
	GIT_TAG        origin/krkrsdl2
)

if(NOT DEFINED EXTRANS_REPO_DIR)
	FetchContent_GetProperties(extrans)
	if(NOT extrans_POPULATED)
		FetchContent_Populate(extrans)
		set(EXTRANS_REPO_DIR ${extrans_SOURCE_DIR} CACHE STRING "")
	endif()
endif()

set(EXTRANS_SRC_DIR ${EXTRANS_REPO_DIR}/extrans)
file(STRINGS ${EXTRANS_SRC_DIR}/src_list/sources.txt EXTRANS_SRC)
list(TRANSFORM EXTRANS_SRC PREPEND ${EXTRANS_SRC_DIR}/)
list(APPEND KRKRSDL2_EXTRA_SRC ${EXTRANS_SRC})
endif()
if(${BUILTIN_PLUGIN_ENABLE_WUTCWF})
FetchContent_Declare(
	wutcwf
	GIT_REPOSITORY https://github.com/krkrsdl2/SamplePlugin.git
	GIT_TAG        origin/krkrsdl2
)

if(NOT DEFINED WUTCWF_REPO_DIR)
	FetchContent_GetProperties(wutcwf)
	if(NOT wutcwf_POPULATED)
		FetchContent_Populate(wutcwf)
		set(WUTCWF_REPO_DIR ${wutcwf_SOURCE_DIR} CACHE STRING "")
	endif()
endif()

set(WUTCWF_SRC_DIR ${WUTCWF_REPO_DIR}/wutcwf)
file(STRINGS ${WUTCWF_SRC_DIR}/src_list/sources.txt WUTCWF_SRC)
list(TRANSFORM WUTCWF_SRC PREPEND ${WUTCWF_SRC_DIR}/)
list(APPEND KRKRSDL2_EXTRA_SRC ${WUTCWF_SRC})
endif()
if(${BUILTIN_PLUGIN_ENABLE_WUVORBIS})
FetchContent_Declare(
	wuvorbis
	GIT_REPOSITORY https://github.com/krkrsdl2/wuvorbis.git
	GIT_TAG        origin/krkrsdl2
)

if(NOT DEFINED WUVORBIS_REPO_DIR)
	FetchContent_GetProperties(wuvorbis)
	if(NOT wuvorbis_POPULATED)
		FetchContent_Populate(wuvorbis)
		set(WUVORBIS_REPO_DIR ${wuvorbis_SOURCE_DIR} CACHE STRING "")
	endif()
endif()

set(WUVORBIS_SRC_DIR ${WUVORBIS_REPO_DIR})
file(STRINGS ${WUVORBIS_SRC_DIR}/src_list/sources.txt WUVORBIS_SRC)
file(STRINGS ${WUVORBIS_SRC_DIR}/src_list/includes.txt WUVORBIS_INCLUDES)
list(TRANSFORM WUVORBIS_SRC PREPEND ${WUVORBIS_SRC_DIR}/)
list(TRANSFORM WUVORBIS_INCLUDES PREPEND ${WUVORBIS_SRC_DIR}/)
list(APPEND KRKRSDL2_EXTRA_SRC ${WUVORBIS_SRC})
list(APPEND KRKRSDL2_EXTRA_INCLUDES ${WUVORBIS_INCLUDES})
endif()
if(${BUILTIN_PLUGIN_ENABLE_FSTAT})
FetchContent_Declare(
	fstat
	GIT_REPOSITORY https://github.com/krkrsdl2/fstat.git
	GIT_TAG        origin/krkrsdl2
)

if(NOT DEFINED FSTAT_REPO_DIR)
	FetchContent_GetProperties(fstat)
	if(NOT fstat_POPULATED)
		FetchContent_Populate(fstat)
		set(FSTAT_REPO_DIR ${fstat_SOURCE_DIR} CACHE STRING "")
	endif()
endif()

set(FSTAT_SRC_DIR ${FSTAT_REPO_DIR})
file(STRINGS ${FSTAT_SRC_DIR}/src_list/sources.txt FSTAT_SRC)
list(TRANSFORM FSTAT_SRC PREPEND ${FSTAT_SRC_DIR}/)
list(APPEND KRKRSDL2_EXTRA_SRC ${FSTAT_SRC})
endif()
if(${BUILTIN_PLUGIN_ENABLE_JSON})
FetchContent_Declare(
	json
	GIT_REPOSITORY https://github.com/krkrsdl2/json.git
	GIT_TAG        origin/krkrsdl2
)

if(NOT DEFINED JSON_REPO_DIR)
	FetchContent_GetProperties(json)
	if(NOT json_POPULATED)
		FetchContent_Populate(json)
		set(JSON_REPO_DIR ${json_SOURCE_DIR} CACHE STRING "")
	endif()
endif()

set(JSON_SRC_DIR ${JSON_REPO_DIR})
file(STRINGS ${JSON_SRC_DIR}/src_list/sources.txt JSON_SRC)
list(TRANSFORM JSON_SRC PREPEND ${JSON_SRC_DIR}/)
list(APPEND KRKRSDL2_EXTRA_SRC ${JSON_SRC})
endif()
if(${BUILTIN_PLUGIN_ENABLE_VARFILE})
FetchContent_Declare(
	varfile
	GIT_REPOSITORY https://github.com/krkrsdl2/varfile.git
	GIT_TAG        origin/krkrsdl2
)

if(NOT DEFINED VARFILE_REPO_DIR)
	FetchContent_GetProperties(varfile)
	if(NOT varfile_POPULATED)
		FetchContent_Populate(varfile)
		set(VARFILE_REPO_DIR ${varfile_SOURCE_DIR} CACHE STRING "")
	endif()
endif()

set(VARFILE_SRC_DIR ${VARFILE_REPO_DIR})
file(STRINGS ${VARFILE_SRC_DIR}/src_list/sources.txt VARFILE_SRC)
list(TRANSFORM VARFILE_SRC PREPEND ${VARFILE_SRC_DIR}/)
list(APPEND KRKRSDL2_EXTRA_SRC ${VARFILE_SRC})
endif()
if(${BUILTIN_PLUGIN_ENABLE_KRGLHWEBP})
FetchContent_Declare(
	krglhwebp
	GIT_REPOSITORY https://github.com/krkrsdl2/krglhwebp.git
	GIT_TAG        origin/krkrsdl2
)

if(NOT DEFINED KRGLHWEBP_REPO_DIR)
	FetchContent_GetProperties(krglhwebp)
	if(NOT krglhwebp_SOURCE_DIR)
		FetchContent_Populate(krglhwebp)
		set(KRGLHWEBP_REPO_DIR ${krglhwebp_SOURCE_DIR} CACHE STRING "")
	endif()
endif()

set(KRGLHWEBP_SRC_DIR ${KRGLHWEBP_REPO_DIR})
file(STRINGS ${KRGLHWEBP_SRC_DIR}/src_list/sources.txt KRGLHWEBP_SRC)
file(STRINGS ${KRGLHWEBP_SRC_DIR}/src_list/includes.txt KRGLHWEBP_INCLUDES)
list(TRANSFORM KRGLHWEBP_SRC PREPEND ${KRGLHWEBP_SRC_DIR}/)
list(TRANSFORM KRGLHWEBP_INCLUDES PREPEND ${KRGLHWEBP_SRC_DIR}/)
list(APPEND KRKRSDL2_EXTRA_SRC ${KRGLHWEBP_SRC})
list(APPEND KRKRSDL2_EXTRA_INCLUDES ${KRGLHWEBP_INCLUDES})
endif()
if(${CMAKE_SYSTEM_NAME} STREQUAL "Emscripten")
FetchContent_Declare(
	kremscripten
	GIT_REPOSITORY https://github.com/krkrsdl2/kremscripten.git
	GIT_TAG        origin/main
)

if(NOT DEFINED KREMSCRIPTEN_REPO_DIR)
	FetchContent_GetProperties(kremscripten)
	if(NOT kremscripten_SOURCE_DIR)
		FetchContent_Populate(kremscripten)
		set(KREMSCRIPTEN_REPO_DIR ${kremscripten_SOURCE_DIR} CACHE STRING "")
	endif()
endif()

set(KREMSCRIPTEN_SRC_DIR ${KREMSCRIPTEN_REPO_DIR}/src)
file(STRINGS ${KREMSCRIPTEN_SRC_DIR}/src_list/sources.txt KRGLHWEBP_SRC)
file(STRINGS ${KREMSCRIPTEN_SRC_DIR}/src_list/includes.txt KRGLHWEBP_INCLUDES)
list(TRANSFORM KREMSCRIPTEN_SRC PREPEND ${KREMSCRIPTEN_SRC_DIR}/)
list(APPEND KRKRSDL2_EXTRA_SRC ${KREMSCRIPTEN_SRC})
endif()

target_sources(${KRKRSDL2_NAME} PRIVATE ${KRKRSDL2_EXTRA_SRC})
target_include_directories(${KRKRSDL2_NAME} PUBLIC ${KRKRSDL2_EXTRA_INCLUDES})
