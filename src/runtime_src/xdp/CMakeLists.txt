if (NOT WIN32)

include_directories(
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${XRT_SRC_DIR}/include/1_2
  )

else()
# = WINDOWS ===================================================================

include_directories(
  ${CMAKE_CURRENT_SOURCE_DIR}
  ${Boost_INCLUDE_DIRS}
  ${XRT_SRC_DIR}/include/1_2
  ${KHRONOS}/include
  )
endif()

# ====== File directory definitions for both Linux and Windows =========

set(XRT_XDP_APPDEBUG_DIR               "${CMAKE_CURRENT_SOURCE_DIR}/appdebug")
set(XRT_XDP_DEBUG_DIR                  "${CMAKE_CURRENT_SOURCE_DIR}/debug")
set(XRT_XDP_PROFILE_DIR                "${CMAKE_CURRENT_SOURCE_DIR}/profile")
set(XRT_XDP_PROFILE_PLUGIN_DIR         "${CMAKE_CURRENT_SOURCE_DIR}/profile/plugin")
set(XRT_XDP_PROFILE_XMA_PLUGIN_DIR     "${CMAKE_CURRENT_SOURCE_DIR}/profile/plugin/xma")
set(XRT_XDP_PROFILE_OCL_PLUGIN_DIR     "${CMAKE_CURRENT_SOURCE_DIR}/profile/plugin/ocl")
set(XRT_XDP_PROFILE_HAL_PLUGIN_DIR     "${CMAKE_CURRENT_SOURCE_DIR}/profile/plugin/hal")
set(XRT_XDP_PROFILE_HAL_API_PLUGIN_DIR "${CMAKE_CURRENT_SOURCE_DIR}/profile/plugin/hal_api_interface")
set(XRT_XDP_PROFILE_COLLECTION_DIR     "${CMAKE_CURRENT_SOURCE_DIR}/profile/collection")
set(XRT_XDP_PROFILE_WRITER_DIR         "${CMAKE_CURRENT_SOURCE_DIR}/profile/writer")
set(XRT_XDP_PROFILE_HAL_WRITER_DIR     "${CMAKE_CURRENT_SOURCE_DIR}/profile/writer/hal")
set(XRT_XDP_PROFILE_CORE_DIR           "${CMAKE_CURRENT_SOURCE_DIR}/profile/core")
set(XRT_XDP_PROFILE_DATABASE_DIR       "${CMAKE_CURRENT_SOURCE_DIR}/profile/database")
set(XRT_XDP_PROFILE_EVENTS_DIR         "${CMAKE_CURRENT_SOURCE_DIR}/profile/database/events")
set(XRT_XDP_PROFILE_DEVICE_DIR         "${CMAKE_CURRENT_SOURCE_DIR}/profile/device")
set(XRT_XDP_PROFILE_HAL_DEVICE_DIR     "${CMAKE_CURRENT_SOURCE_DIR}/profile/device/hal_device")

file(GLOB XRT_XDP_APPDEBUG_FILES
  "${XRT_XDP_APPDEBUG_DIR}/*.h"
  "${XRT_XDP_APPDEBUG_DIR}/*.cpp"
  )

if (NOT WIN32)
  set_source_files_properties (${XRT_XDP_APPDEBUG_FILES} PROPERTIES COMPILE_FLAGS -g)
endif()

file(GLOB XRT_XDP_DEBUG_FILES
  "${XRT_XDP_DEBUG_DIR}/*.h"
  "${XRT_XDP_DEBUG_DIR}/*.cpp"
  )

file(GLOB XRT_XDP_PROFILE_FILES
  "${XRT_XDP_PROFILE_DIR}/*.h"
  "${XRT_XDP_PROFILE_DIR}/*.cpp"
  "${XRT_XDP_PROFILE_COLLECTION_DIR}/*.h"
  "${XRT_XDP_PROFILE_COLLECTION_DIR}/*.cpp"
  "${XRT_XDP_PROFILE_WRITER_DIR}/*.h"
  "${XRT_XDP_PROFILE_WRITER_DIR}/*.cpp"
  "${XRT_XDP_PROFILE_CORE_DIR}/*.h"
  "${XRT_XDP_PROFILE_CORE_DIR}/*.cpp"
  "${XRT_XDP_PROFILE_DATABASE_DIR}/*.h"
  "${XRT_XDP_PROFILE_DATABASE_DIR}/*.cpp"
  "${XRT_XDP_PROFILE_EVENTS_DIR}/*.h"
  "${XRT_XDP_PROFILE_EVENTS_DIR}/*.cpp"
  "${XRT_XDP_PROFILE_DEVICE_DIR}/*.h"
  "${XRT_XDP_PROFILE_DEVICE_DIR}/*.cpp"
  "${XRT_XDP_PROFILE_PLUGIN_DIR}/*.h"
  "${XRT_XDP_PROFILE_PLUGIN_DIR}/*.cpp"
  )

file(GLOB XRT_XDP_FILES
  "${CMAKE_CURRENT_SOURCE_DIR}/*.h"
  "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp"
  )

  file(GLOB XRT_XDP_PROFILE_OCL_PLUGIN_FILES
  "${XRT_XDP_PROFILE_PLUGIN_DIR}/*.h"
  "${XRT_XDP_PROFILE_OCL_PLUGIN_DIR}/*.h"
  "${XRT_XDP_PROFILE_OCL_PLUGIN_DIR}/*.cpp"
  )

file(GLOB XRT_XDP_PROFILE_XMA_PLUGIN_FILES
  "${XRT_XDP_PROFILE_XMA_PLUGIN_DIR}/*.h"
  "${XRT_XDP_PROFILE_XMA_PLUGIN_DIR}/*.cpp"
  )

file(GLOB XRT_XDP_PROFILE_HAL_PLUGIN_FILES
  "${XRT_XDP_PROFILE_PLUGIN_DIR}/*.h"
  "${XRT_XDP_PROFILE_HAL_DEVICE_DIR}/*.h"
  "${XRT_XDP_PROFILE_HAL_DEVICE_DIR}/*.cpp"
  "${XRT_XDP_PROFILE_HAL_PLUGIN_DIR}/*.h"
  "${XRT_XDP_PROFILE_HAL_PLUGIN_DIR}/*.cpp"
  "${XRT_XDP_PROFILE_HAL_WRITER_DIR}/*.h"
  "${XRT_XDP_PROFILE_HAL_WRITER_DIR}/*.cpp"
  )

file(GLOB XRT_XDP_PROFILE_HAL_INTERFACE_PLUGIN_FILES
  "${XRT_XDP_PROFILE_HAL_DEVICE_DIR}/*.h"
  "${XRT_XDP_PROFILE_HAL_DEVICE_DIR}/*.cpp"
  "${XRT_XDP_PROFILE_HAL_API_PLUGIN_DIR}/*.h"
  "${XRT_XDP_PROFILE_HAL_API_PLUGIN_DIR}/*.cpp"
  )

set(XRT_XDP_ALL_SRC
  ${XRT_XDP_PROFILE_FILES}
  ${XRT_XDP_FILES}
  )

if (NOT WIN32)

# ====== Linux XDP Libraries ======
add_library(xdp SHARED ${XRT_XDP_ALL_SRC})
add_library(oclxdp SHARED ${XRT_XDP_PROFILE_OCL_PLUGIN_FILES})
add_library(xdp_hal_plugin SHARED ${XRT_XDP_PROFILE_HAL_PLUGIN_FILES})
add_library(xdp_hal_api_interface_plugin SHARED ${XRT_XDP_PROFILE_HAL_INTERFACE_PLUGIN_FILES})
add_library(xdp_debug_plugin SHARED ${XRT_XDP_DEBUG_FILES})
add_library(xdp_appdebug_plugin SHARED ${XRT_XDP_APPDEBUG_FILES})

add_dependencies(xdp xrt_core xilinxopencl)
add_dependencies(oclxdp xdp xilinxopencl)
add_dependencies(xdp_hal_plugin xdp)
add_dependencies(xdp_hal_api_interface_plugin xdp)
add_dependencies(xdp_debug_plugin xrt_core xilinxopencl)
add_dependencies(xdp_appdebug_plugin xrt_core xilinxopencl)

target_link_libraries(xdp xrt_core xilinxopencl)
target_link_libraries(oclxdp xdp xilinxopencl)
target_link_libraries(xdp_hal_plugin xdp)
target_link_libraries(xdp_hal_api_interface_plugin xdp)
target_link_libraries(xdp_debug_plugin xrt_core xilinxopencl)
target_link_libraries(xdp_appdebug_plugin xrt_core xilinxopencl)

set_target_properties(xdp PROPERTIES VERSION ${XRT_VERSION_STRING} SOVERSION ${XRT_SOVERSION})
set_target_properties(oclxdp PROPERTIES VERSION ${XRT_VERSION_STRING} SOVERSION ${XRT_SOVERSION})
set_target_properties(xdp_hal_plugin PROPERTIES VERSION ${XRT_VERSION_STRING} SOVERSION ${XRT_SOVERSION})
set_target_properties(xdp_hal_api_interface_plugin PROPERTIES VERSION ${XRT_VERSION_STRING} SOVERSION ${XRT_SOVERSION})
set_target_properties(xdp_debug_plugin PROPERTIES VERSION ${XRT_VERSION_STRING} SOVERSION ${XRT_SOVERSION})
set_target_properties(xdp_appdebug_plugin PROPERTIES VERSION ${XRT_VERSION_STRING} SOVERSION ${XRT_SOVERSION})

install (TARGETS xdp oclxdp xdp_hal_plugin xdp_hal_api_interface_plugin xdp_debug_plugin xdp_appdebug_plugin LIBRARY DESTINATION ${XRT_INSTALL_DIR}/lib)

install (FILES "${XRT_XDP_PROFILE_XMA_PLUGIN_DIR}/xma_profile.h" DESTINATION ${XRT_INSTALL_INCLUDE_DIR})

# Only install these files for PCIe device for now, which is .
if (${XRT_NATIVE_BUILD} STREQUAL "yes")
set (APPDEBUG_INSTALL_PREFIX "/opt/xilinx/xrt/share/appdebug")
install (FILES
  ${XRT_XDP_APPDEBUG_DIR}/appdebug.py
  ${XRT_XDP_APPDEBUG_DIR}/appdebugint.py
  DESTINATION ${APPDEBUG_INSTALL_PREFIX}
)
endif()

else()
# = WINDOWS ===================================================================

add_library(xdpobj OBJECT ${XRT_XDP_PROFILE_OCL_PLUGIN_FILES})

add_library(xdp SHARED ${XRT_XDP_ALL_SRC}
  $<TARGET_OBJECTS:xdpobj>
)

add_dependencies(xdp xrt_core xilinxopencl)
target_link_libraries (xdp xrt_core xilinxopencl)

install(TARGETS xdp
  ARCHIVE DESTINATION ${XRT_INSTALL_DIR}/lib
  LIBRARY DESTINATION ${XRT_INSTALL_DIR}/lib
  RUNTIME DESTINATION ${XRT_INSTALL_DIR}/bin)

# oclxdp

add_library(oclxdp SHARED ""
  $<TARGET_OBJECTS:xdpobj>
  )
add_dependencies(oclxdp xdp xilinxopencl)
target_link_libraries (oclxdp xdp xilinxopencl)

install(TARGETS oclxdp
  ARCHIVE DESTINATION ${XRT_INSTALL_DIR}/lib
  LIBRARY DESTINATION ${XRT_INSTALL_DIR}/lib
  RUNTIME DESTINATION ${XRT_INSTALL_DIR}/bin)

# xdp_hal_plugin

add_library(xdp_hal_plugin_obj OBJECT ${XRT_XDP_PROFILE_HAL_PLUGIN_FILES})

add_library(xdp_hal_plugin SHARED
  $<TARGET_OBJECTS:xdp_hal_plugin_obj>
  )

add_dependencies(xdp_hal_plugin xdp xrt_core)

target_link_libraries(xdp_hal_plugin xdp xrt_core)

#set_target_properties(xdp_hal_plugin PROPERTIES VERSION ${XRT_VERSION_STRING}
#  SOVERSION ${XRT_SOVERSION})

install(TARGETS xdp_hal_plugin
  ARCHIVE DESTINATION ${XRT_INSTALL_DIR}/lib
  LIBRARY DESTINATION ${XRT_INSTALL_DIR}/lib
  RUNTIME DESTINATION ${XRT_INSTALL_DIR}/bin)

# xdp_hal_api_interface_plugin

add_library(xdp_hal_api_interface_plugin_obj OBJECT ${XRT_XDP_PROFILE_HAL_INTERFACE_PLUGIN_FILES})

add_library(xdp_hal_api_interface_plugin SHARED
  $<TARGET_OBJECTS:xdp_hal_api_interface_plugin_obj>
  )

add_dependencies(xdp_hal_api_interface_plugin xdp)

target_link_libraries(xdp_hal_api_interface_plugin xdp)

#set_target_properties(xdp_hal_api_interface_plugin PROPERTIES VERSION ${XRT_VERSION_STRING}
#  SOVERSION ${XRT_SOVERSION})

install(TARGETS xdp_hal_api_interface_plugin
  ARCHIVE DESTINATION ${XRT_INSTALL_DIR}/lib
  LIBRARY DESTINATION ${XRT_INSTALL_DIR}/lib
  RUNTIME DESTINATION ${XRT_INSTALL_DIR}/bin)

# install (FILES "${XRT_XDP_PROFILE_XMA_PLUGIN_DIR}/xma_profile.h" DESTINATION ${XRT_INSTALL_INCLUDE_DIR})

# Only install these files for PCIe device for now, which is .
if (${XRT_NATIVE_BUILD} STREQUAL "yes")
#set (APPDEBUG_INSTALL_PREFIX "/opt/xilinx/xrt/share/appdebug")
set (APPDEBUG_INSTALL_PREFIX "${XRT_INSTALL_DIR}/share/appdebug")
install (FILES
  ${XRT_XDP_APPDEBUG_DIR}/appdebug.py
  ${XRT_XDP_APPDEBUG_DIR}/appdebugint.py
  DESTINATION ${APPDEBUG_INSTALL_PREFIX}
)
endif()
endif()
