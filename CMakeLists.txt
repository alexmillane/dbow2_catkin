cmake_minimum_required(VERSION 2.8.3)
project(dbow3_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple(ALL_DEPS_REQUIRED)

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(dbow3_src
  GIT_REPOSITORY git@github.com:rmsalinas/DBow3.git
  CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${CATKIN_DEVEL_PREFIX}
  BUILD_COMMAND CXXFLAGS=-i${CATKIN_DEVEL_PREFIX}/include make
  INSTALL_COMMAND make install
)
ExternalProject_CatkinInstall(dbow3_src)

cs_add_library(${PROJECT_NAME} SHARED src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} dbow3_src)
target_link_libraries(${PROJECT_NAME} 
  ${CATKIN_DEVEL_PREFIX}/lib/libDBoW3${CMAKE_SHARED_LIBRARY_SUFFIX})

cs_install()
cs_export(
  INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
  CFG_EXTRAS dbow3-extras.cmake)
