cmake_minimum_required(VERSION 2.8.3)
project(trac_ik_lib)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
    set(CMAKE_CXX_FLAGS "-std=c++11 ${CMAKE_CXX_FLAGS}")
endif()

find_package(catkin REQUIRED
  COMPONENTS
    kdl_parser
    roscpp
    urdf
)

# In case the NLopt library is not found during build see the instruction in README.me 
# After that, you will be able to see why adding the path to the CMAKE_PREFIX_PATH as the
# example below makes sense
#list(APPEND CMAKE_PREFIX_PATH "c:/path")

find_package(Boost REQUIRED COMPONENTS date_time)
find_package(NLopt CONFIG REQUIRED)
find_package(Eigen3 REQUIRED)

catkin_package(
  CATKIN_DEPENDS
    kdl_parser
    roscpp
    urdf
  DEPENDS
    Boost
    # purposefully not including nlopt here, see earlier note
  INCLUDE_DIRS
    include
  LIBRARIES
    trac_ik
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${Eigen_INCLUDE_DIRS}
  ${NLopt_INCLUDE_DIRS}
  ${Boost_INCLUDE_DIRS}
)

add_library(trac_ik
  src/kdl_tl.cpp
  src/nlopt_ik.cpp
  src/trac_ik.cpp)
target_link_libraries(trac_ik
  ${catkin_LIBRARIES}
  ${Boost_LIBRARIES}
  NLopt::nlopt)

install(DIRECTORY include/
  DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
)

install(TARGETS trac_ik
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)