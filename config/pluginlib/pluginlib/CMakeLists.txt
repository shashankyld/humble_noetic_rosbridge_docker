cmake_minimum_required(VERSION 3.0.2)
project(pluginlib)

find_package(catkin REQUIRED COMPONENTS class_loader rosconsole roslib cmake_modules)
find_package(Boost REQUIRED COMPONENTS filesystem)
find_package(TinyXML2 REQUIRED)

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS class_loader rosconsole roslib
  DEPENDS Boost TinyXML2
)

if(CATKIN_ENABLE_TESTING)
  include_directories(include ${catkin_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS} ${TinyXML2_INCLUDE_DIRS})

  add_library(test_plugins EXCLUDE_FROM_ALL SHARED test/test_plugins.cpp)
  target_link_libraries(test_plugins ${class_loader_LIBRARIES})

  catkin_add_gtest(${PROJECT_NAME}_utest test/utest.cpp)
  if(TARGET ${PROJECT_NAME}_utest)
    target_link_libraries(${PROJECT_NAME}_utest ${TinyXML2_LIBRARIES} ${catkin_LIBRARIES} ${Boost_LIBRARIES})
    add_dependencies(${PROJECT_NAME}_utest test_plugins)
  endif()

  include(CheckCXXCompilerFlag)
  check_cxx_compiler_flag("-std=c++17" COMPILER_SUPPORTS_CXX17)
  if(COMPILER_SUPPORTS_CXX17)
    catkin_add_gtest(${PROJECT_NAME}_unique_ptr_test test/unique_ptr_test.cpp)
    if(TARGET ${PROJECT_NAME}_unique_ptr_test)
      target_link_libraries(${PROJECT_NAME}_unique_ptr_test ${TinyXML2_LIBRARIES} ${catkin_LIBRARIES} ${Boost_LIBRARIES})
      set_target_properties(${PROJECT_NAME}_unique_ptr_test PROPERTIES COMPILE_FLAGS -std=c++17 LINK_FLAGS -std=c++17)
      add_dependencies(${PROJECT_NAME}_unique_ptr_test test_plugins)
    endif()
  endif()

endif()

install(DIRECTORY include/pluginlib/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})

install(PROGRAMS scripts/pluginlib_headers_migration.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
