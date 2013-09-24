set(CUKE_SOURCES
    ContextManager.cpp
    CukeCommands.cpp
    CukeEngine.cpp
    CukeEngineImpl.cpp
    StepManager.cpp
    HookRegistrar.cpp
    Regex.cpp
    Scenario.cpp
    Table.cpp
    Tag.cpp
    connectors/wire/WireServer.cpp
    connectors/wire/WireProtocol.cpp
    connectors/wire/WireProtocolCommands.cpp
    main.cpp
)

if(GTEST_FOUND)
    include_directories(${GTEST_INCLUDE_DIRS})
    list(APPEND CUKE_SOURCES drivers/GTestDriver.cpp)
    list(APPEND CUKE_LINK_LIBRARIES pthread ${GTEST_LIBRARIES})
endif()

if(CPPSPEC_FOUND)
    include_directories(${CPPSPEC_INCLUDE_DIRS})
    list(APPEND CUKE_SOURCES drivers/CppSpecDriver.cpp)
endif()

if(Boost_UNIT_TEST_FRAMEWORK_FOUND)
    list(APPEND CUKE_SOURCES drivers/BoostDriver.cpp)
    list(APPEND CUKE_LINK_LIBRARIES ${Boost_UNIT_TEST_FRAMEWORK_LIBRARY})
endif()

if(CMAKE_EXTRA_GENERATOR OR MSVC_IDE)
    message(STATUS "Adding header files to project")
    file(GLOB_RECURSE CUKE_HEADERS "${CUKE_INCLUDE_DIR}/cucumber-cpp/*.hpp")
    if(MSVC_IDE)
        source_group("Header Files" FILES ${CUKE_HEADERS})
    endif()
    list(APPEND CUKE_SOURCES ${CUKE_HEADERS})
endif()

add_library(cucumber-cpp SHARED ${CUKE_SOURCES})
target_link_libraries(cucumber-cpp ${CUKE_LINK_LIBRARIES})

install(DIRECTORY ${CUKE_INCLUDE_DIR}/cucumber-cpp DESTINATION include)
install(DIRECTORY ${CUKE_INCLUDE_DIR}/json_spirit DESTINATION include)
install(TARGETS cucumber-cpp
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)
