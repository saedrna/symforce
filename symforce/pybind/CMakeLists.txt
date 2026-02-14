# ----------------------------------------------------------------------------
# SymForce - Copyright 2022, Skydio, Inc.
# This source code is under the Apache 2.0 license found in the LICENSE file.
# ----------------------------------------------------------------------------

# ==============================================================================
# Third Party Dependencies
# ==============================================================================

include(FetchContent)

# Ensure Python3 is found with Development.Module BEFORE pybind11.
# ament_cmake already sets Python3_FOUND (Interpreter only), which causes
# pybind11NewTools.cmake to skip its own find_package(Python) and then fail
# because python3_add_library is missing without the Development component.
find_package(Python3 COMPONENTS Interpreter Development.Module Development.Embed REQUIRED)

set(Python_FIND_VIRTUALENV ONLY)
find_package(pybind11 REQUIRED)

# ==============================================================================
# SymForce Targets
# ==============================================================================

# ------------------------------------------------------------------------------
# cc_sym

pybind11_add_module(
  cc_sym
  SHARED
  cc_factor.cc
  cc_key.cc
  cc_linearization.cc
  cc_logger.cc
  cc_optimization_stats.cc
  cc_optimizer.cc
  cc_slam.cc
  cc_sym.cc
  cc_values.cc
  sym_type_casters.cc
)

target_compile_options(cc_sym PRIVATE ${SYMFORCE_COMPILE_OPTIONS})

if(SYMFORCE_PY_EXTENSION_MODULE_OUTPUT_PATH)
  set_target_properties(cc_sym PROPERTIES
    LIBRARY_OUTPUT_DIRECTORY ${SYMFORCE_PY_EXTENSION_MODULE_OUTPUT_PATH}
  )
endif()

target_link_libraries(
  cc_sym
  PRIVATE
  symforce_opt
  symforce_gen
  symforce_slam
)

# TODO(aaron): Should there be an install action here?  This is currently installed by setup.py if
# you do `pip install .`
