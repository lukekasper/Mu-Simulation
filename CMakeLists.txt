cmake_minimum_required(VERSION 3.1 FATAL_ERROR)

project(MATHUSLA_MU_SIM)

set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/modules)

find_package(Geant4 REQUIRED ui_all vis_all)
find_package(Pythia8 REQUIRED)

include(${Geant4_USE_FILE})

set(SOURCES
    src/simulation.cc

    src/action/ActionInitialization.cc
    src/action/EventAction.cc
    src/action/PrimaryGeneratorAction.cc
    src/action/RunAction.cc
    src/action/StackingAction.cc
    src/action/SteppingAction.cc
    src/action/TrackingAction.cc

    src/detector/DetectorConstruction.cc
    src/detector/DetectorMessenger.cc
    src/detector/RegionInfo.cc
    src/detector/TrackerSD.cc

    src/tracking/TrackerHit.cc
    src/tracking/Trajectory.cc
    src/tracking/TrajectoryPoint.cc

    src/physics/PhysicsList.cc
)

set(SCRIPTS
    scripts/simulation.in

    scripts/startup.mac
    scripts/init_vis.mac
    scripts/vis.mac
    scripts/gui.mac

    scripts/run1.mac
    scripts/run2.mac
)

add_executable(simulation ${SOURCES})
target_link_libraries(simulation ${Geant4_LIBRARIES})

target_include_directories(simulation PUBLIC
    $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:${CMAKE_SOURCE_DIR}/include>)

install(DIRECTORY scripts DESTINATION bin)
install(TARGETS simulation DESTINATION bin)