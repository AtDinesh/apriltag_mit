cmake_minimum_required(VERSION 3.5)
project(apriltag_mit)

set(USE_ROS FALSE)
set(CMAKE_CXX_STANDARD 11)

find_package(Eigen3 REQUIRED)
find_package(OpenCV REQUIRED)
include(FindPkgConfig)
pkg_search_module(OPENCV opencv>=2.3 opencv-2.3.1 opencv-3.0.0)
if(OPENCV_FOUND)
  message(STATUS "opencv found: ${OpenCV_INCLUDE_DIRS}")
else(NOT OPENCV_FOUND)
  message(FATAL_ERROR "opencv_include_dirs not found")
endif(OPENCV_FOUND)

include_directories(include ${OpenCV_INCLUDE_DIRS} ${EIGEN3_INCLUDE_DIR})

if(USE_ROS)
  find_package(catkin REQUIRED COMPONENTS roslib)
  catkin_package(DEPENDS
                 OpenCV
                 EIGEN3
                 INCLUDE_DIRS
                 include
                 LIBRARIES
                 ${PROJECT_NAME})

  include_directories(${catkin_INCLUDE_DIRS})
  file(GLOB CC_FILES ${PROJECT_SOURCE_DIR}/src/*.cc)
  add_library(${PROJECT_NAME} SHARED ${CC_FILES})
  target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBRARIES})

  if(CATKIN_ENABLE_TESTING)
    catkin_add_gtest(test_apriltag_mit test/test_apriltag_mit.cpp)
    target_link_libraries(test_apriltag_mit ${catkin_LIBRARIES} ${PROJECT_NAME})
  endif()

  install(TARGETS ${PROJECT_NAME}
          ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
          LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
          RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
  install(DIRECTORY include/${PROJECT_NAME}/
          DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})
else()
  message(STATUS "Source:  " ${PROJECT_SOURCE_DIR})
  message(STATUS "Binary:  " ${PROJECT_BINARY_DIR})

  #add subdirectories
  add_subdirectory(include)
  
  # define library target
  #add_library(apriltag_mit ${header})
  file(GLOB CC_FILES ${PROJECT_SOURCE_DIR}/src/*.cc)
  add_library(${PROJECT_NAME} SHARED ${CC_FILES} ${header})
  target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBRARIES})
  #target_include_directories(${PROJECT_NAME} PUBLIC ${PROJECT_SOURCE_DIR}/include)  #throws an error

  set(header_path "${PROJECT_SOURCE_DIR}/include/apriltag_mit")
	set(CMAKE_INSTALL_PREFIX /usr/local)
  set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
  set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)
	message(STATUS "Library: " ${LIBRARY_OUTPUT_PATH})
  link_directories(${PROJECT_SOURCE_DIR}/lib)

  set(include_dest "include/${PROJECT_NAME}")
  set(main_lib_dest "lib/${PROJECT_NAME}")
  
  install(TARGETS ${PROJECT_NAME} EXPORT ${PROJECT_NAME} DESTINATION "${main_lib_dest}")
  install(FILES ${header} DESTINATION "${include_dest}")
  install(FILES apriltag_mit-config.cmake DESTINATION ${main_lib_dest})
  install(EXPORT ${PROJECT_NAME} DESTINATION "${main_lib_dest}")

endif()
