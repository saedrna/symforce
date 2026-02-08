cmake_minimum_required(VERSION 3.19)  # For interface library headers
project(skymarshal)

add_library(skymarshal_core INTERFACE include/lcm/lcm_coretypes.h include/lcm/lcm_cpptypes.hpp include/lcm/lcm_reflection.hpp include/lcm/lcm_reflection_eigen.hpp)
target_include_directories(skymarshal_core SYSTEM INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)
install(DIRECTORY include/ DESTINATION include)
install(TARGETS skymarshal_core EXPORT SymForceTargets)
