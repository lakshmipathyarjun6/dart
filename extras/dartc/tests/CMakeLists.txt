# Copyright (c) 2011-2021, The DART development contributors
# All rights reserved.
#
# The list of contributors can be found at:
#   https://github.com/dartsim/dart/blob/main/LICENSE
#
# This file is provided under the "BSD-style" License

include_directories(BEFORE SYSTEM ${DART_SOURCE_DIR}/tests/gtest/include)
include_directories(BEFORE SYSTEM ${DART_SOURCE_DIR}/tests/gtest)

function(dartc_add_test test_type target_name) # ARGN for source files

#   dart_property_add(DART_${test_type}_TESTS ${target_name})

  if(${ARGC} GREATER 2)
    set(sources ${ARGN})
  else()
    set(sources "${target_name}.cpp")
  endif()

  add_executable(${target_name} ${sources})
  add_test(${target_name} ${target_name})

  if(MSVC)
    target_link_libraries(${target_name}
        dartc
        optimized gtest debug gtestd
        optimized gtest_main debug gtest_maind
    )
  else()
    target_link_libraries(${target_name} dartc gtest gtest_main)
  endif()

endfunction()

dartc_add_test("dartc" test_dartc_skeleton)
dartc_add_test("dartc" test_dartc_simulation)
