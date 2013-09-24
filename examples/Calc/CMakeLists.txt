project(Calc)

include_directories(${CUKE_INCLUDE_DIRS} src)

add_library(Calc src/Calculator)

if(GTEST_FOUND)
    include_directories(${GTEST_INCLUDE_DIRS})
    add_executable(GTestCalculatorSteps features/step_definitions/GTestCalculatorSteps)
    target_link_libraries(GTestCalculatorSteps Calc ${GTEST_LIBRARIES} ${CUKE_LIBRARIES})
    list(APPEND CALC_EXAMPLES GTestCalculatorSteps)
endif()

if(CPPSPEC_FOUND)
    include_directories(${CPPSPEC_INCLUDE_DIRS})
    add_executable(CppSpecCalculatorSteps features/step_definitions/CppSpecCalculatorSteps)
    target_link_libraries(CppSpecCalculatorSteps Calc ${CPPSPEC_LIBRARY} ${CUKE_LIBRARIES})
    list(APPEND CALC_EXAMPLES CppSpecCalculatorSteps)
endif()

if(Boost_UNIT_TEST_FRAMEWORK_FOUND)
    include_directories(${Boost_INCLUDE_DIRS})
    add_executable(BoostCalculatorSteps features/step_definitions/BoostCalculatorSteps)
    target_link_libraries(BoostCalculatorSteps Calc ${Boost_UNIT_TEST_FRAMEWORK_LIBRARY} ${CUKE_LIBRARIES})
    list(APPEND CALC_EXAMPLES BoostCalculatorSteps)
endif()

install(DIRECTORY features DESTINATION ${CUCUMBER_CPP_DOC_SUBDIR}/examples/Calc)

install(TARGETS ${CALC_EXAMPLES}
  RUNTIME DESTINATION ${CUCUMBER_CPP_DOC_SUBDIR}/examples/Calc
)
