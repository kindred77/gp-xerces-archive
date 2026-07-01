#=============================================================================
# CMake build for Xerces-C++ test programs
#=============================================================================

# ---- include paths ----
set(TEST_INCLUDES
  ${CMAKE_SOURCE_DIR}/src
  ${PROJECT_BINARY_DIR}/src
)

# ---- DOMTest ----
add_executable(DOMTest
  src/DOM/DOMTest/DTest.cpp
  src/DOM/DOMTest/DTest.h
)
target_include_directories(DOMTest PRIVATE ${TEST_INCLUDES})
target_link_libraries(DOMTest PRIVATE xerces-c)

# ---- DOMMemTest ----
add_executable(DOMMemTest
  src/DOM/DOMMemTest/DOMMemTest.cpp
)
target_include_directories(DOMMemTest PRIVATE ${TEST_INCLUDES})
target_link_libraries(DOMMemTest PRIVATE xerces-c)

# ---- Normalizer ----
add_executable(Normalizer
  src/DOM/Normalizer/Normalizer.cpp
  src/DOM/Normalizer/Normalizer.hpp
)
target_include_directories(Normalizer PRIVATE ${TEST_INCLUDES})
target_link_libraries(Normalizer PRIVATE xerces-c)

# ---- RangeTest ----
add_executable(RangeTest
  src/DOM/RangeTest/RangeTest.cpp
)
target_include_directories(RangeTest PRIVATE ${TEST_INCLUDES})
target_link_libraries(RangeTest PRIVATE xerces-c)

# ---- DOMTraversalTest ----
add_executable(DOMTraversalTest
  src/DOM/Traversal/Traversal.cpp
)
target_include_directories(DOMTraversalTest PRIVATE ${TEST_INCLUDES})
target_link_libraries(DOMTraversalTest PRIVATE xerces-c)

# ---- DOMTypeInfoTest ----
add_executable(DOMTypeInfoTest
  src/DOM/TypeInfo/TypeInfo.cpp
  src/DOM/TypeInfo/TypeInfo.hpp
)
target_include_directories(DOMTypeInfoTest PRIVATE ${TEST_INCLUDES})
target_link_libraries(DOMTypeInfoTest PRIVATE xerces-c)

# ---- EncodingTest ----
add_executable(EncodingTest
  src/EncodingTest/EncodingTest.cpp
)
target_include_directories(EncodingTest PRIVATE ${TEST_INCLUDES})
target_link_libraries(EncodingTest PRIVATE xerces-c)

# ---- InitTermTest ----
add_executable(InitTermTest
  src/InitTermTest/InitTermTest.cpp
  src/InitTermTest/InitTermTest.hpp
)
target_include_directories(InitTermTest PRIVATE ${TEST_INCLUDES})
target_link_libraries(InitTermTest PRIVATE xerces-c)

# ---- MemHandlerTest ----
add_executable(MemHandlerTest
  src/MemHandlerTest/MemoryMonitor.cpp
  src/MemHandlerTest/MemoryMonitor.hpp
  src/MemHandlerTest/SimpleValueHashTableOf.hpp
)
target_include_directories(MemHandlerTest PRIVATE ${TEST_INCLUDES})
target_link_libraries(MemHandlerTest PRIVATE xerces-c)

# ---- NetAccessorTest ----
add_executable(NetAccessorTest
  src/NetAccessorTest/NetAccessorTest.cpp
)
target_include_directories(NetAccessorTest PRIVATE ${TEST_INCLUDES})
target_link_libraries(NetAccessorTest PRIVATE xerces-c)

# ---- ThreadTest ----
add_executable(ThreadTest
  src/ThreadTest/ThreadTest.cpp
)
target_include_directories(ThreadTest PRIVATE ${TEST_INCLUDES})
target_link_libraries(ThreadTest PRIVATE xerces-c)

# ---- XSerializerTest ----
add_executable(XSerializerTest
  src/XSerializerTest/XSerializerHandlers.cpp
  src/XSerializerTest/XSerializerHandlers.hpp
  src/XSerializerTest/XSerializerTest.cpp
  src/XSerializerTest/XSerializerTest.hpp
)
target_include_directories(XSerializerTest PRIVATE ${TEST_INCLUDES})
target_link_libraries(XSerializerTest PRIVATE xerces-c)

# ---- XSTSHarness ----
add_executable(XSTSHarness
  src/XSTSHarness/XSTSHarness.cpp
  src/XSTSHarness/XSTSHarness.hpp
  src/XSTSHarness/XSTSHarnessHandlers.cpp
  src/XSTSHarness/XSTSHarnessHandlers.hpp
  src/XSTSHarness/XMLHarnessHandlers.cpp
  src/XSTSHarness/XMLHarnessHandlers.hpp
)
target_include_directories(XSTSHarness PRIVATE ${TEST_INCLUDES})
target_link_libraries(XSTSHarness PRIVATE xerces-c)

# ---- XSValueTest ----
add_executable(XSValueTest
  src/XSValueTest/XSValueTest.cpp
  src/XSValueTest/XSValueTest.hpp
)
target_include_directories(XSValueTest PRIVATE ${TEST_INCLUDES})
target_link_libraries(XSValueTest PRIVATE xerces-c)

message(STATUS "  Tests enabled")
