cmake_minimum_required(VERSION 3.8...3.18)

project(Simulation)

find_package(Geant4 REQUIRED ui_all vis_all)
if(NOT Geant4_FOUND)
    set(Geant4_DIR /cvmfs/geant4.cern.ch/geant4/latest-ref/x86_64-centos7-gcc8-optdeb-MT/lib64/cmake/Geant4/)
endif()

# Add the GEANT4 include directories to your project
include(${Geant4_USE_FILE})
include_directories(${Geant4_INCLUDE_DIRS})

file(GLOB sources ${PROJECT_SOURCE_DIR}/*.cc)
file(GLOB headers ${PROJECT_SOURCE_DIR}/*.hh)

add_executable(box1 box1.cc ${sources} ${headers})
target_link_libraries(box1 ${Geant4_LIBRARIES})

file(GLOB MACRO_FILES
    "*.mac"
    )
file(COPY ${MACRO_FILES} DESTINATION ${PROJECT_BINARY_DIR})

file(GLOB DATA_FILES
    "*.dat"
    )
file(COPY ${DATA_FILES} DESTINATION ${PROJECT_BINARY_DIR})

add_custom_target(Simulation DEPENDS box1)