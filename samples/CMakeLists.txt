#=============================================================================
# CMake build for Xerces-C++ sample programs
#=============================================================================

# ---- include paths ----
set(SAMPLE_INCLUDES
  ${CMAKE_SOURCE_DIR}/src
  ${PROJECT_BINARY_DIR}/src
)

# ---- each sample links against xerces-c ----
link_directories(${CMAKE_BINARY_DIR}/src)

# ---- CreateDOMDocument ----
add_executable(CreateDOMDocument
  src/CreateDOMDocument/CreateDOMDocument.cpp
)
target_include_directories(CreateDOMDocument PRIVATE ${SAMPLE_INCLUDES})
target_link_libraries(CreateDOMDocument PRIVATE xerces-c)

# ---- DOMCount ----
add_executable(DOMCount
  src/DOMCount/DOMCount.cpp
  src/DOMCount/DOMCount.hpp
)
target_include_directories(DOMCount PRIVATE ${SAMPLE_INCLUDES})
target_link_libraries(DOMCount PRIVATE xerces-c)

# ---- DOMPrint ----
add_executable(DOMPrint
  src/DOMPrint/DOMPrint.cpp
  src/DOMPrint/DOMPrintErrorHandler.cpp
  src/DOMPrint/DOMPrintErrorHandler.hpp
  src/DOMPrint/DOMPrintFilter.cpp
  src/DOMPrint/DOMPrintFilter.hpp
  src/DOMPrint/DOMTreeErrorReporter.cpp
  src/DOMPrint/DOMTreeErrorReporter.hpp
)
target_include_directories(DOMPrint PRIVATE ${SAMPLE_INCLUDES})
target_link_libraries(DOMPrint PRIVATE xerces-c)

# ---- EnumVal ----
add_executable(EnumVal
  src/EnumVal/EnumVal.cpp
)
target_include_directories(EnumVal PRIVATE ${SAMPLE_INCLUDES})
target_link_libraries(EnumVal PRIVATE xerces-c)

# ---- MemParse ----
add_executable(MemParse
  src/MemParse/MemParse.cpp
  src/MemParse/MemParse.hpp
  src/MemParse/MemParseHandlers.cpp
  src/MemParse/MemParseHandlers.hpp
)
target_include_directories(MemParse PRIVATE ${SAMPLE_INCLUDES})
target_link_libraries(MemParse PRIVATE xerces-c)

# ---- PParse ----
add_executable(PParse
  src/PParse/PParse.cpp
  src/PParse/PParse.hpp
  src/PParse/PParseHandlers.cpp
  src/PParse/PParseHandlers.hpp
)
target_include_directories(PParse PRIVATE ${SAMPLE_INCLUDES})
target_link_libraries(PParse PRIVATE xerces-c)

# ---- PSVIWriter ----
add_executable(PSVIWriter
  src/PSVIWriter/PSVIWriter.cpp
  src/PSVIWriter/PSVIWriter.hpp
  src/PSVIWriter/PSVIWriterHandlers.cpp
  src/PSVIWriter/PSVIWriterHandlers.hpp
)
target_include_directories(PSVIWriter PRIVATE ${SAMPLE_INCLUDES})
target_link_libraries(PSVIWriter PRIVATE xerces-c)

# ---- Redirect ----
add_executable(Redirect
  src/Redirect/Redirect.cpp
  src/Redirect/Redirect.hpp
  src/Redirect/RedirectHandlers.cpp
  src/Redirect/RedirectHandlers.hpp
)
target_include_directories(Redirect PRIVATE ${SAMPLE_INCLUDES})
target_link_libraries(Redirect PRIVATE xerces-c)

# ---- SAX2Count ----
add_executable(SAX2Count
  src/SAX2Count/SAX2Count.cpp
  src/SAX2Count/SAX2Count.hpp
  src/SAX2Count/SAX2CountHandlers.cpp
  src/SAX2Count/SAX2CountHandlers.hpp
)
target_include_directories(SAX2Count PRIVATE ${SAMPLE_INCLUDES})
target_link_libraries(SAX2Count PRIVATE xerces-c)

# ---- SAX2Print ----
add_executable(SAX2Print
  src/SAX2Print/SAX2FilterHandlers.cpp
  src/SAX2Print/SAX2FilterHandlers.hpp
  src/SAX2Print/SAX2Print.cpp
  src/SAX2Print/SAX2Print.hpp
  src/SAX2Print/SAX2PrintHandlers.cpp
  src/SAX2Print/SAX2PrintHandlers.hpp
)
target_include_directories(SAX2Print PRIVATE ${SAMPLE_INCLUDES})
target_link_libraries(SAX2Print PRIVATE xerces-c)

# ---- SAXCount ----
add_executable(SAXCount
  src/SAXCount/SAXCount.cpp
  src/SAXCount/SAXCount.hpp
  src/SAXCount/SAXCountHandlers.cpp
  src/SAXCount/SAXCountHandlers.hpp
)
target_include_directories(SAXCount PRIVATE ${SAMPLE_INCLUDES})
target_link_libraries(SAXCount PRIVATE xerces-c)

# ---- SAXPrint ----
add_executable(SAXPrint
  src/SAXPrint/SAXPrint.cpp
  src/SAXPrint/SAXPrint.hpp
  src/SAXPrint/SAXPrintHandlers.cpp
  src/SAXPrint/SAXPrintHandlers.hpp
)
target_include_directories(SAXPrint PRIVATE ${SAMPLE_INCLUDES})
target_link_libraries(SAXPrint PRIVATE xerces-c)

# ---- SCMPrint ----
add_executable(SCMPrint
  src/SCMPrint/SCMPrint.cpp
)
target_include_directories(SCMPrint PRIVATE ${SAMPLE_INCLUDES})
target_link_libraries(SCMPrint PRIVATE xerces-c)

# ---- SEnumVal ----
add_executable(SEnumVal
  src/SEnumVal/SEnumVal.cpp
)
target_include_directories(SEnumVal PRIVATE ${SAMPLE_INCLUDES})
target_link_libraries(SEnumVal PRIVATE xerces-c)

# ---- StdInParse ----
add_executable(StdInParse
  src/StdInParse/StdInParse.cpp
  src/StdInParse/StdInParse.hpp
  src/StdInParse/StdInParseHandlers.cpp
  src/StdInParse/StdInParseHandlers.hpp
)
target_include_directories(StdInParse PRIVATE ${SAMPLE_INCLUDES})
target_link_libraries(StdInParse PRIVATE xerces-c)

# ---- XInclude ----
add_executable(XInclude
  src/XInclude/XInclude.cpp
  src/XInclude/XInclude.hpp
)
target_include_directories(XInclude PRIVATE ${SAMPLE_INCLUDES})
target_link_libraries(XInclude PRIVATE xerces-c)

message(STATUS "  Samples enabled")
