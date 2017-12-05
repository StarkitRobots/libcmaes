project(libcmaes)
cmake_minimum_required(VERSION 2.8)

find_package(catkin REQUIRED COMPONENTS
  eigen
  )

catkin_package(
  INCLUDE_DIRS .
  LIBRARIES cmaes
  CATKIN_DEPENDS eigen
)

set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)

option (ENABLE_SURROGATE
    "Enable surrogate" OFF)

option (BUILD_EXAMPLES
    "Build examples" OFF)

#Include sources directory
include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/src)
include_directories(${catkin_INCLUDE_DIRS})

#Enable C++11
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

#Sources files
set(SOURCES_FILES
    src/acovarianceupdate.cc
    src/cmasolutions.cc
    src/covarianceupdate.cc
    src/ipopcmastrategy.cc
    src/vdcmaupdate.cc
    src/bipopcmastrategy.cc
    src/cmastopcriteria.cc
    src/errstats.cc
    src/pwq_bound_strategy.cc
    src/cmaparameters.cc
    src/cmastrategy.cc
    src/esostrategy.cc
)

if (ENABLE_SURROGATE)
    set (SOURCES_FILES
        ${SOURCE_FILES}
        src/surrogatestrategy.cc
        )
endif ()

#Special flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fopenmp")

#Build Code as static library
add_library(cmaes SHARED ${SOURCES_FILES})
target_link_libraries(cmaes)

if (BUILD_EXAMPLES)
    add_executable(sample-code examples/sample-code.cc)
    target_link_libraries(sample-code cmaes)
endif ()
