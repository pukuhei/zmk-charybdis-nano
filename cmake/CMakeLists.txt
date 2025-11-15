if(NOT DEFINED ZEPHYR_MODULES)
  return()
endif()

set(_pmw3610_module_path "")
foreach(module_path ${ZEPHYR_MODULES})
  if(module_path MATCHES "zmk-pmw3610-driver$")
    set(_pmw3610_module_path ${module_path})
    break()
  endif()
endforeach()

if(NOT _pmw3610_module_path)
  message(STATUS "zmkcharybdis: zmk-pmw3610-driver module not present, skipping compat patch.")
  return()
endif()

set(_pmw3610_driver_src "${_pmw3610_module_path}/src/pmw3610.c")
if(NOT EXISTS "${_pmw3610_driver_src}")
  message(WARNING "zmkcharybdis: ${_pmw3610_driver_src} not found, automouse compat patch skipped.")
  return()
endif()

file(READ "${_pmw3610_driver_src}" _pmw3610_driver_contents)

# Already patched if the signature includes the locking argument.
if(_pmw3610_driver_contents MATCHES "zmk_keymap_layer_activate\\([^,]+,[^)]*\\)")
  message(STATUS "zmkcharybdis: zmk-pmw3610-driver already uses locking parameter; no patch needed.")
  return()
endif()

set(_pmw3610_driver_patched "${_pmw3610_driver_contents}")
string(REPLACE "zmk_keymap_layer_activate(AUTOMOUSE_LAYER);"
               "zmk_keymap_layer_activate(AUTOMOUSE_LAYER, false);"
               _pmw3610_driver_patched "${_pmw3610_driver_patched}")
string(REPLACE "zmk_keymap_layer_deactivate(AUTOMOUSE_LAYER);"
               "zmk_keymap_layer_deactivate(AUTOMOUSE_LAYER, false);"
               _pmw3610_driver_patched "${_pmw3610_driver_patched}")

if(_pmw3610_driver_patched STREQUAL _pmw3610_driver_contents)
  message(WARNING "zmkcharybdis: automouse layer calls not found; pmw3610 driver patch not applied.")
  return()
endif()

file(WRITE "${_pmw3610_driver_src}" "${_pmw3610_driver_patched}")
message(STATUS "zmkcharybdis: patched zmk-pmw3610-driver automouse layer calls for locking API.")
