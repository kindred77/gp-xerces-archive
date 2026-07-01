#=============================================================================
# CMake build for the Xerces-C++ library
#=============================================================================

# ---- internal replacement sources (from AC_REPLACE_FUNCS) ----
# These are used when the platform lacks stricmp, strnicmp, towlower, towupper
set(_replacement_sources
  ${CMAKE_CURRENT_SOURCE_DIR}/stricmp.c
  ${CMAKE_CURRENT_SOURCE_DIR}/strnicmp.c
  ${CMAKE_CURRENT_SOURCE_DIR}/towlower.c
  ${CMAKE_CURRENT_SOURCE_DIR}/towupper.c
)

set(_compiled_replacements "")
if(NOT HAVE_STRICMP)
  list(APPEND _compiled_replacements ${CMAKE_CURRENT_SOURCE_DIR}/stricmp.c)
endif()
if(NOT HAVE_STRNICMP)
  list(APPEND _compiled_replacements ${CMAKE_CURRENT_SOURCE_DIR}/strnicmp.c)
endif()
if(NOT HAVE_TOWLOWER)
  list(APPEND _compiled_replacements ${CMAKE_CURRENT_SOURCE_DIR}/towlower.c)
endif()
if(NOT HAVE_TOWUPPER)
  list(APPEND _compiled_replacements ${CMAKE_CURRENT_SOURCE_DIR}/towupper.c)
endif()

# ---- core library sources ----
# These directories contain only core Xerces code, not the pluggable backends
file(GLOB_RECURSE _dom_sources       ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/dom/*.cpp)
file(GLOB_RECURSE _framework_sources ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/framework/*.cpp)
file(GLOB_RECURSE _internal_sources  ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/internal/*.cpp)
file(GLOB_RECURSE _parsers_sources   ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/parsers/*.cpp)
file(GLOB_RECURSE _sax_sources       ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/sax/*.cpp)
file(GLOB_RECURSE _sax2_sources      ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/sax2/*.cpp)
file(GLOB_RECURSE _xinclude_sources  ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/xinclude/*.cpp)

# validators
file(GLOB_RECURSE _validators_sources
  ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/validators/common/*.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/validators/datatype/*.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/validators/DTD/*.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/validators/schema/*.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/validators/schema/identity/*.cpp
)

# util (core only, not pluggable backends)
file(GLOB _util_core_sources
  ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/*.cpp)
file(GLOB_RECURSE _util_regx_sources
  ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/regx/*.cpp)
set(_util_sources ${_util_core_sources} ${_util_regx_sources})

# ---- remove NLS from core (it holds .xml/.dtd files, not C++) ----

# ---- conditional transcoder sources ----
if(TRANSCODER STREQUAL "icu")
  file(GLOB_RECURSE _transcoder_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/Transcoders/ICU/*.cpp
  )
elseif(TRANSCODER STREQUAL "gnuiconv")
  file(GLOB_RECURSE _transcoder_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/Transcoders/IconvGNU/*.cpp
  )
elseif(TRANSCODER STREQUAL "iconv")
  file(GLOB_RECURSE _transcoder_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/Transcoders/Iconv/*.cpp
  )
elseif(TRANSCODER STREQUAL "macosunicodeconverter")
  file(GLOB_RECURSE _transcoder_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/Transcoders/MacOSUnicodeConverter/*.cpp
  )
elseif(TRANSCODER STREQUAL "windows")
  file(GLOB_RECURSE _transcoder_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/Transcoders/Win32/*.cpp
  )
else()
  message(FATAL_ERROR "Unknown transcoder: ${TRANSCODER}")
endif()

# ---- conditional netaccessor sources ----
if(NETACCESSOR STREQUAL "curl")
  file(GLOB_RECURSE _netaccessor_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/NetAccessors/Curl/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/NetAccessors/BinHTTPInputStreamCommon.cpp
  )
elseif(NETACCESSOR STREQUAL "winsock")
  file(GLOB_RECURSE _netaccessor_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/NetAccessors/WinSock/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/NetAccessors/BinHTTPInputStreamCommon.cpp
  )
elseif(NETACCESSOR STREQUAL "socket")
  file(GLOB_RECURSE _netaccessor_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/NetAccessors/Socket/*.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/NetAccessors/BinHTTPInputStreamCommon.cpp
  )
elseif(NETACCESSOR STREQUAL "cfurl")
  file(GLOB_RECURSE _netaccessor_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/NetAccessors/MacOSURLAccessCF/*.cpp
  )
elseif(NETACCESSOR STREQUAL "")
  # No network accessor selected
  set(_netaccessor_sources "")
else()
  message(FATAL_ERROR "Unknown netaccessor: ${NETACCESSOR}")
endif()

# ---- conditional msgloader sources ----
if(MSGLOADER STREQUAL "inmemory")
  file(GLOB_RECURSE _msgloader_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/MsgLoaders/InMemory/*.cpp
  )
elseif(MSGLOADER STREQUAL "icu")
  file(GLOB_RECURSE _msgloader_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/MsgLoaders/ICU/*.cpp
  )
elseif(MSGLOADER STREQUAL "iconv")
  file(GLOB_RECURSE _msgloader_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/MsgLoaders/MsgCatalog/*.cpp
  )
else()
  message(FATAL_ERROR "Unknown msgloader: ${MSGLOADER}")
endif()

# ---- conditional mutexmgr sources ----
if(MUTEXMGR STREQUAL "windows")
  file(GLOB_RECURSE _mutexmgr_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/MutexManagers/WindowsMutexMgr.cpp
  )
elseif(MUTEXMGR STREQUAL "posix")
  file(GLOB_RECURSE _mutexmgr_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/MutexManagers/PosixMutexMgr.cpp
  )
elseif(MUTEXMGR STREQUAL "nothread")
  file(GLOB_RECURSE _mutexmgr_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/MutexManagers/NoThreadMutexMgr.cpp
  )
else()
  message(FATAL_ERROR "Unknown mutexmgr: ${MUTEXMGR}")
endif()

# ---- conditional filemgr sources ----
if(FILEMGR STREQUAL "windows")
  file(GLOB_RECURSE _filemgr_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/FileManagers/WindowsFileMgr.cpp
  )
elseif(FILEMGR STREQUAL "posix")
  file(GLOB_RECURSE _filemgr_sources
    ${CMAKE_CURRENT_SOURCE_DIR}/xercesc/util/FileManagers/PosixFileMgr.cpp
  )
else()
  message(FATAL_ERROR "Unknown filemgr: ${FILEMGR}")
endif()

# ---- collect all library sources ----
set(XERCES_ALL_SOURCES
  ${_compiled_replacements}
  ${_dom_sources}
  ${_framework_sources}
  ${_internal_sources}
  ${_parsers_sources}
  ${_sax_sources}
  ${_sax2_sources}
  ${_validators_sources}
  ${_xinclude_sources}
  ${_util_sources}
  ${_transcoder_sources}
  ${_netaccessor_sources}
  ${_msgloader_sources}
  ${_mutexmgr_sources}
  ${_filemgr_sources}
)

# ---- build the library ----
if(XERCES_STATIC_LIBRARY)
  add_library(xerces-c STATIC ${XERCES_ALL_SOURCES})
else()
  add_library(xerces-c SHARED ${XERCES_ALL_SOURCES})
endif()

# ---- set soversion ----
set_target_properties(xerces-c PROPERTIES
  VERSION   ${PROJECT_VERSION}
  SOVERSION ${XERCES_SOVERSION}
  CXX_STANDARD 11
  CXX_STANDARD_REQUIRED ON
  CXX_EXTENSIONS OFF
)

# ---- include directories ----
target_include_directories(xerces-c
  PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<BUILD_INTERFACE:${PROJECT_BINARY_DIR}/src>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
  PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${PROJECT_BINARY_DIR}/src
)

# ---- required defines ----
target_compile_definitions(xerces-c PRIVATE XERCES_BUILDING_LIBRARY)
if(NOT XERCES_STATIC_LIBRARY)
  target_compile_definitions(xerces-c PUBLIC DLL_EXPORT)
  target_compile_definitions(xerces-c PRIVATE _CRT_SECURE_NO_WARNINGS _SCL_SECURE_NO_WARNINGS)
endif()

# ---- component selection defines ----
if(TRANSCODER STREQUAL "windows")
  target_compile_definitions(xerces-c PRIVATE -DXERCES_USE_TRANSCODER_WINDOWS=1)
elseif(TRANSCODER STREQUAL "icu")
  target_compile_definitions(xerces-c PRIVATE -DXERCES_USE_TRANSCODER_ICU=1)
elseif(TRANSCODER STREQUAL "gnuiconv")
  target_compile_definitions(xerces-c PRIVATE -DXERCES_USE_TRANSCODER_GNUICONV=1)
elseif(TRANSCODER STREQUAL "iconv")
  target_compile_definitions(xerces-c PRIVATE -DXERCES_USE_TRANSCODER_ICONV=1)
elseif(TRANSCODER STREQUAL "macosunicodeconverter")
  target_compile_definitions(xerces-c PRIVATE -DXERCES_USE_TRANSCODER_MACOSUNICODECONVERTER=1)
endif()

if(NETACCESSOR STREQUAL "curl")
  target_compile_definitions(xerces-c PRIVATE -DXERCES_USE_NETACCESSOR_CURL=1)
elseif(NETACCESSOR STREQUAL "winsock")
  target_compile_definitions(xerces-c PRIVATE -DXERCES_USE_NETACCESSOR_WINSOCK=1)
elseif(NETACCESSOR STREQUAL "socket")
  target_compile_definitions(xerces-c PRIVATE -DXERCES_USE_NETACCESSOR_SOCKET=1)
elseif(NETACCESSOR STREQUAL "cfurl")
  target_compile_definitions(xerces-c PRIVATE -DXERCES_USE_NETACCESSOR_CFURL=1)
endif()

if(MSGLOADER STREQUAL "inmemory")
  target_compile_definitions(xerces-c PRIVATE -DXERCES_USE_MSGLOADER_INMEMORY=1)
elseif(MSGLOADER STREQUAL "icu")
  target_compile_definitions(xerces-c PRIVATE -DXERCES_USE_MSGLOADER_ICU=1)
elseif(MSGLOADER STREQUAL "iconv")
  target_compile_definitions(xerces-c PRIVATE -DXERCES_USE_MSGLOADER_ICONV=1)
endif()

if(MUTEXMGR STREQUAL "windows")
  target_compile_definitions(xerces-c PRIVATE -DXERCES_USE_MUTEXMGR_WINDOWS=1)
elseif(MUTEXMGR STREQUAL "posix")
  target_compile_definitions(xerces-c PRIVATE -DXERCES_USE_MUTEXMGR_POSIX=1)
elseif(MUTEXMGR STREQUAL "nothread")
  target_compile_definitions(xerces-c PRIVATE -DXERCES_USE_MUTEXMGR_NOTHREAD=1)
endif()

if(FILEMGR STREQUAL "windows")
  target_compile_definitions(xerces-c PRIVATE -DXERCES_USE_FILEMGR_WINDOWS=1)
elseif(FILEMGR STREQUAL "posix")
  target_compile_definitions(xerces-c PRIVATE -DXERCES_USE_FILEMGR_POSIX=1)
endif()

# ---- feature detection defines (config.h equivalents) ----
# These were checked in the root CMakeLists.txt
target_compile_definitions(xerces-c PRIVATE
  $<$<BOOL:${HAVE_FCNTL_H}>:HAVE_FCNTL_H=1>
  $<$<BOOL:${HAVE_FLOAT_H}>:HAVE_FLOAT_H=1>
  $<$<BOOL:${HAVE_INTTYPES_H}>:HAVE_INTTYPES_H=1>
  $<$<BOOL:${HAVE_LOCALE_H}>:HAVE_LOCALE_H=1>
  $<$<BOOL:${HAVE_MEMORY_H}>:HAVE_MEMORY_H=1>
  $<$<BOOL:${HAVE_NETDB_H}>:HAVE_NETDB_H=1>
  $<$<BOOL:${HAVE_NL_TYPES_H}>:HAVE_NL_TYPES_H=1>
  $<$<BOOL:${HAVE_STDDEF_H}>:HAVE_STDDEF_H=1>
  $<$<BOOL:${HAVE_STDINT_H}>:HAVE_STDINT_H=1>
  $<$<BOOL:${HAVE_STDLIB_H}>:HAVE_STDLIB_H=1>
  $<$<BOOL:${HAVE_STRING_H}>:HAVE_STRING_H=1>
  $<$<BOOL:${HAVE_STRINGS_H}>:HAVE_STRINGS_H=1>
  $<$<BOOL:${HAVE_SYS_PARAM_H}>:HAVE_SYS_PARAM_H=1>
  $<$<BOOL:${HAVE_SYS_TIMEB_H}>:HAVE_SYS_TIMEB_H=1>
  $<$<BOOL:${HAVE_SYS_TIME_H}>:HAVE_SYS_TIME_H=1>
  $<$<BOOL:${HAVE_SYS_TYPES_H}>:HAVE_SYS_TYPES_H=1>
  $<$<BOOL:${HAVE_UNISTD_H}>:HAVE_UNISTD_H=1>
  $<$<BOOL:${HAVE_WCHAR_H}>:HAVE_WCHAR_H=1>
  $<$<BOOL:${HAVE_WCTYPE_H}>:HAVE_WCTYPE_H=1>
  $<$<BOOL:${HAVE_ARPA_INET_H}>:HAVE_ARPA_INET_H=1>
  $<$<BOOL:${HAVE_NETINET_IN_H}>:HAVE_NETINET_IN_H=1>
  $<$<BOOL:${HAVE_WINSOCK2_H}>:HAVE_WINSOCK2_H=1>
  $<$<BOOL:${HAVE_DLFCN_H}>:HAVE_DLFCN_H=1>
  $<$<BOOL:${HAVE_ERRNO_H}>:HAVE_ERRNO_H=1>
  $<$<BOOL:${HAVE_LANGINFO_H}>:HAVE_LANGINFO_H=1>
  $<$<BOOL:${HAVE_LIMITS_H}>:HAVE_LIMITS_H=1>
  $<$<BOOL:${HAVE_GETCWD}>:HAVE_GETCWD=1>
  $<$<BOOL:${HAVE_PATHCONF}>:HAVE_PATHCONF=1>
  $<$<BOOL:${HAVE_REALPATH}>:HAVE_REALPATH=1>
  $<$<BOOL:${HAVE_CLOCK_GETTIME}>:HAVE_CLOCK_GETTIME=1>
  $<$<BOOL:${HAVE_FTIME}>:HAVE_FTIME=1>
  $<$<BOOL:${HAVE_GETTIMEOFDAY}>:HAVE_GETTIMEOFDAY=1>
  $<$<BOOL:${HAVE_MEMMOVE}>:HAVE_MEMMOVE=1>
  $<$<BOOL:${HAVE_MEMSET}>:HAVE_MEMSET=1>
  $<$<BOOL:${HAVE_NL_LANGINFO}>:HAVE_NL_LANGINFO=1>
  $<$<BOOL:${HAVE_SETLOCALE}>:HAVE_SETLOCALE=1>
  $<$<BOOL:${HAVE_LOCALECONV}>:HAVE_LOCALECONV=1>
  $<$<BOOL:${HAVE_STRCASECMP}>:HAVE_STRCASECMP=1>
  $<$<BOOL:${HAVE_STRNCASECMP}>:HAVE_STRNCASECMP=1>
  $<$<BOOL:${HAVE_STRICMP}>:HAVE_STRICMP=1>
  $<$<BOOL:${HAVE_STRNICMP}>:HAVE_STRNICMP=1>
  $<$<BOOL:${HAVE_STRCHR}>:HAVE_STRCHR=1>
  $<$<BOOL:${HAVE_STRDUP}>:HAVE_STRDUP=1>
  $<$<BOOL:${HAVE_STRRCHR}>:HAVE_STRRCHR=1>
  $<$<BOOL:${HAVE_STRSTR}>:HAVE_STRSTR=1>
  $<$<BOOL:${HAVE_STRTOL}>:HAVE_STRTOL=1>
  $<$<BOOL:${HAVE_STRTOUL}>:HAVE_STRTOUL=1>
  $<$<BOOL:${HAVE_TOWUPPER}>:HAVE_TOWUPPER=1>
  $<$<BOOL:${HAVE_TOWLOWER}>:HAVE_TOWLOWER=1>
  $<$<BOOL:${HAVE_MBLEN}>:HAVE_MBLEN=1>
  $<$<BOOL:${HAVE_WCSUPR}>:HAVE_WCSUPR=1>
  $<$<BOOL:${HAVE_WCSLWR}>:HAVE_WCSLWR=1>
  $<$<BOOL:${HAVE_WCSNICMP}>:HAVE_WCSNICMP=1>
  $<$<BOOL:${HAVE_WCSICMP}>:HAVE_WCSICMP=1>
  $<$<BOOL:${HAVE_MBRLEN}>:HAVE_MBRLEN=1>
  $<$<BOOL:${HAVE_WCSRTOMBS}>:HAVE_WCSRTOMBS=1>
  $<$<BOOL:${HAVE_MBSRTOWCS}>:HAVE_MBSRTOWCS=1>
  $<$<BOOL:${ICONV_USES_CONST_POINTER}>:ICONV_USES_CONST_POINTER=1>
  $<$<BOOL:${HAVE_PATH_MAX}>:HAVE_PATH_MAX=1>
  $<$<BOOL:${HAVE_SYS_SOCKET_H}>:HAVE_SYS_SOCKET_H=1>
  $<$<BOOL:${HAVE_ICONV_H}>:HAVE_ICONV_H=1>
  $<$<BOOL:${HAVE_ICONV}>:HAVE_ICONV=1>
  $<$<BOOL:${HAVE_ICONV_OPEN}>:HAVE_ICONV_OPEN=1>
  $<$<BOOL:${HAVE_ICONV_CLOSE}>:HAVE_ICONV_CLOSE=1>
  $<$<BOOL:${SIZEOF_WCHAR_T}>:SIZEOF_WCHAR_T=${SIZEOF_WCHAR_T}>
  $<$<BOOL:${SIZEOF_INT}>:SIZEOF_INT=${SIZEOF_INT}>
  $<$<BOOL:${SIZEOF_LONG}>:SIZEOF_LONG=${SIZEOF_LONG}>
  $<$<BOOL:${SIZEOF_LONG_LONG}>:SIZEOF_LONG_LONG=${SIZEOF_LONG_LONG}>
  $<$<BOOL:${SIZEOF_SHORT}>:SIZEOF_SHORT=${SIZEOF_SHORT}>
  $<$<BOOL:${SIZEOF___INT64}>:SIZEOF___INT64=${SIZEOF___INT64}>
  $<$<BOOL:${HAVE_CORESERVICES_CORESERVICES_H}>:HAVE_CORESERVICES_CORESERVICES_H=1>
  $<$<BOOL:${HAVE_GETADDRINFO}>:HAVE_GETADDRINFO=1>
  $<$<BOOL:${HAVE_GETHOSTBYADDR}>:HAVE_GETHOSTBYADDR=1>
  $<$<BOOL:${HAVE_GETHOSTBYNAME}>:HAVE_GETHOSTBYNAME=1>
  $<$<BOOL:${HAVE_SOCKET}>:HAVE_SOCKET=1>
  $<$<BOOL:${HAVE_ENDIAN_H}>:HAVE_ENDIAN_H=1>
  $<$<BOOL:${HAVE_MACHINE_ENDIAN_H}>:HAVE_MACHINE_ENDIAN_H=1>
  $<$<BOOL:${HAVE_ARPA_NAMESER_COMPAT_H}>:HAVE_ARPA_NAMESER_COMPAT_H=1>
)

# ---- SSE2 flags ----
if(sse2 AND XERCES_HAVE_SSE2_INTRINSIC)
  target_compile_definitions(xerces-c PRIVATE
    XERCES_HAVE_SSE2_INTRINSIC=1
  )
  if(MSVC)
    target_compile_options(xerces-c PRIVATE /arch:SSE2)
  else()
    target_compile_options(xerces-c PRIVATE -msse2)
  endif()
endif()

# ---- Windows-specific settings ----
if(WIN32)
  # Disable deprecation warnings for standard C functions
  target_compile_definitions(xerces-c PRIVATE _CRT_SECURE_NO_WARNINGS _SCL_SECURE_NO_WARNINGS)

  # Link Windows libraries
  target_link_libraries(xerces-c PRIVATE
    ws2_32
    # For XERCES_USE_NETACCESSOR_WINSOCK or XERCES_USE_TRANSCODER_WINDOWS
    $<$<STREQUAL:${NETACCESSOR},winsock>:ws2_32>
  )
endif()

# ---- link ICU if needed ----
if(TRANSCODER STREQUAL "icu" OR MSGLOADER STREQUAL "icu")
  find_package(ICU REQUIRED COMPONENTS uc)
  target_include_directories(xerces-c PRIVATE ${ICU_INCLUDE_DIRS})
  target_link_libraries(xerces-c PRIVATE ${ICU_LIBRARIES})
endif()

# ---- link pthread on Unix ----
if(MUTEXMGR STREQUAL "posix")
  find_package(Threads REQUIRED)
  target_link_libraries(xerces-c PRIVATE Threads::Threads)
endif()

# ---- link CoreServices on macOS for MacOSUnicodeConverter ----
if(APPLE AND (TRANSCODER STREQUAL "macosunicodeconverter" OR NETACCESSOR STREQUAL "cfurl"))
  find_library(CORE_SERVICES_FW CoreServices)
  if(CORE_SERVICES_FW)
    target_link_libraries(xerces-c PRIVATE "${CORE_SERVICES_FW}")
  endif()
endif()

# ---- link curl if needed ----
if(NETACCESSOR STREQUAL "curl")
  find_package(CURL REQUIRED)
  target_include_directories(xerces-c PRIVATE ${CURL_INCLUDE_DIRS})
  target_link_libraries(xerces-c PRIVATE ${CURL_LIBRARIES})
endif()

# ---- msg catalog for iconv/msgloader ----
if(MSGLOADER STREQUAL "iconv")
  find_package(Iconv REQUIRED)
  target_include_directories(xerces-c PRIVATE ${Iconv_INCLUDE_DIRS})
  target_link_libraries(xerces-c PRIVATE ${Iconv_LIBRARIES})
endif()

# ---- gnuiconv needs iconv ----
if(TRANSCODER STREQUAL "gnuiconv")
  find_package(Iconv REQUIRED)
  target_include_directories(xerces-c PRIVATE ${Iconv_INCLUDE_DIRS})
  target_link_libraries(xerces-c PRIVATE ${Iconv_LIBRARIES})
endif()

message(STATUS "  Source files: ${_dom_sources}...")
