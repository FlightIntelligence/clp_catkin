cmake_minimum_required(VERSION 2.8.3)
project(clp_catkin)
add_compile_options(-std=c++11)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

find_package(catkin REQUIRED)

catkin_package(
  INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
  CFG_EXTRAS clp-extras.cmake
)

include(ExternalProject)

ExternalProject_Add(clp_src
  URL ${CMAKE_CURRENT_SOURCE_DIR}/Clp-1.16.11.tgz
  CONFIGURE_COMMAND cd ../clp_src && ./configure -C --prefix=${CATKIN_DEVEL_PREFIX}
  BUILD_COMMAND cd ../clp_src && make -j8
  INSTALL_COMMAND cd ../clp_src && make install -j8
  )

add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} clp_src)

target_link_libraries(${PROJECT_NAME}
  ${CATKIN_DEVEL_PREFIX}/lib/libClp${CMAKE_SHARED_LIBRARY_SUFFIX}
  ${CATKIN_DEVEL_PREFIX}/lib/libClpSolver${CMAKE_SHARED_LIBRARY_SUFFIX}
  ${CATKIN_DEVEL_PREFIX}/lib/libCoinUtils${CMAKE_SHARED_LIBRARY_SUFFIX}
  ${CATKIN_DEVEL_PREFIX}/lib/libOsi${CMAKE_SHARED_LIBRARY_SUFFIX}
  ${CATKIN_DEVEL_PREFIX}/lib/libOsiClp${CMAKE_SHARED_LIBRARY_SUFFIX}
  )