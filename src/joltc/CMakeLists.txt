# Define target name
if (DOUBLE_PRECISION)
    set (TARGET_NAME joltc_double)
else()
    set (TARGET_NAME joltc)
endif ()

set(SOURCE_FILES
    joltc.h
    joltc.cpp
    joltc_assert.cpp
	joltc.c
)

if (JPH_BUILD_SHARED)
	add_library(${TARGET_NAME} SHARED ${SOURCE_FILES})
else()
	add_library(${TARGET_NAME} ${SOURCE_FILES})
endif()

target_include_directories(${TARGET_NAME} PUBLIC
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
	${JoltPhysics_SOURCE_DIR}/..
)
target_link_libraries(${TARGET_NAME} PUBLIC Jolt)

if(JPH_BUILD_SHARED)
	set_property(TARGET Jolt PROPERTY POSITION_INDEPENDENT_CODE ON)
    set_property(TARGET ${TARGET_NAME} PROPERTY POSITION_INDEPENDENT_CODE ON)

    target_compile_definitions(${TARGET_NAME} PRIVATE JPH_SHARED_LIBRARY_BUILD=1)
	set_property(TARGET ${TARGET_NAME} PROPERTY POSITION_INDEPENDENT_CODE ON)
endif()

install(TARGETS ${TARGET_NAME}
    EXPORT ${TARGET_NAME}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
