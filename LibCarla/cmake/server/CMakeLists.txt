cmake_minimum_required(VERSION 3.5.1)
project(libcarla-server)

# Install libc++ shared libraries.
file(GLOB LibCXX_Libraries "${LLVM_LIB_PATH}/libc++*")
install(FILES ${LibCXX_Libraries} DESTINATION lib)

# Install rpclib.
install(DIRECTORY "${RPCLIB_INCLUDE_PATH}/rpc" DESTINATION include)
file(GLOB libcarla_carla_rpclib "${RPCLIB_LIB_PATH}/*.*")
install(FILES ${libcarla_carla_rpclib} DESTINATION lib)

# Install headers.

install(DIRECTORY "${libcarla_source_path}/compiler" DESTINATION include)

file(GLOB libcarla_carla_headers "${libcarla_source_path}/carla/*.h")
install(FILES ${libcarla_carla_headers} DESTINATION include/carla)

file(GLOB libcarla_carla_geom_headers "${libcarla_source_path}/carla/geom/*.h")
install(FILES ${libcarla_carla_geom_headers} DESTINATION include/carla/geom)

file(GLOB libcarla_carla_profiler_headers "${libcarla_source_path}/carla/profiler/*.h")
install(FILES ${libcarla_carla_profiler_headers} DESTINATION include/carla/profiler)

file(GLOB libcarla_carla_rpc_headers "${libcarla_source_path}/carla/rpc/*.h")
install(FILES ${libcarla_carla_rpc_headers} DESTINATION include/carla/rpc)

file(GLOB libcarla_carla_sensor_headers "${libcarla_source_path}/carla/sensor/*.h")
install(FILES ${libcarla_carla_sensor_headers} DESTINATION include/carla/sensor)

file(GLOB libcarla_carla_sensor_data_headers "${libcarla_source_path}/carla/sensor/data/*.h")
install(FILES ${libcarla_carla_sensor_data_headers} DESTINATION include/carla/sensor/data)

file(GLOB libcarla_carla_sensor_s11n_headers "${libcarla_source_path}/carla/sensor/s11n/*.h")
install(FILES ${libcarla_carla_sensor_s11n_headers} DESTINATION include/carla/sensor/s11n)

file(GLOB libcarla_carla_streaming_headers "${libcarla_source_path}/carla/streaming/*.h")
install(FILES ${libcarla_carla_streaming_headers} DESTINATION include/carla/streaming)

file(GLOB libcarla_carla_streaming_detail_headers "${libcarla_source_path}/carla/streaming/detail/*.h")
install(FILES ${libcarla_carla_streaming_detail_headers} DESTINATION include/carla/streaming/detail)

file(GLOB libcarla_carla_streaming_detail_tcp_headers "${libcarla_source_path}/carla/streaming/detail/tcp/*.h")
install(FILES ${libcarla_carla_streaming_detail_tcp_headers} DESTINATION include/carla/streaming/detail/tcp)

file(GLOB libcarla_carla_streaming_low_level_headers "${libcarla_source_path}/carla/streaming/low_level/*.h")
install(FILES ${libcarla_carla_streaming_low_level_headers} DESTINATION include/carla/streaming/low_level)

file(GLOB libcarla_carla_road_headers "${libcarla_source_path}/carla/road/*.h")
install(FILES ${libcarla_carla_road_headers} DESTINATION include/carla/road)

file(GLOB libcarla_carla_road_element_headers "${libcarla_source_path}/carla/road/element/*.h")
install(FILES ${libcarla_carla_road_element_headers} DESTINATION include/carla/road/element)

file(GLOB libcarla_carla_road_element_cephes_headers "${libcarla_source_path}/carla/road/element/cephes/*.h")
install(FILES ${libcarla_carla_road_element_cephes_headers} DESTINATION include/carla/road/element/cephes)

file(GLOB libcarla_carla_opendrive "${libcarla_source_path}/carla/opendrive/*.h")
install(FILES ${libcarla_carla_opendrive} DESTINATION include/carla/opendrive)

file(GLOB libcarla_carla_opendrive_parser "${libcarla_source_path}/carla/opendrive/parser/*.h")
install(FILES ${libcarla_carla_opendrive_parser} DESTINATION include/carla/opendrive/parser)

file(GLOB libcarla_carla_opendrive_parser_pugixml "${libcarla_source_path}/carla/opendrive/parser/pugixml/*.hpp")
install(FILES ${libcarla_carla_opendrive_parser_pugixml} DESTINATION include/carla/opendrive/parser/pugixml)

install(DIRECTORY "${BOOST_INCLUDE_PATH}/boost" DESTINATION include)

if(WIN32)
  file(GLOB boostlib_date_time "${BOOST_LIB_PATH}/libboost_date_time-*-mt-*.lib")
  install(FILES ${boostlib_date_time} DESTINATION lib)

  file(GLOB boostlib_system "${BOOST_LIB_PATH}/libboost_system-*-mt-*.lib")
  install(FILES ${boostlib_system} DESTINATION lib)
endif()

# carla_server library.

file(GLOB libcarla_server_sources
    "${libcarla_source_path}/carla/*.h"
    "${libcarla_source_path}/carla/Buffer.cpp"
    "${libcarla_source_path}/carla/Exception.cpp"
    "${libcarla_source_path}/carla/geom/*.cpp"
    "${libcarla_source_path}/carla/geom/*.h"
    "${libcarla_source_path}/carla/opendrive/*.cpp"
    "${libcarla_source_path}/carla/opendrive/*.h"
    "${libcarla_source_path}/carla/opendrive/parser/*.cpp"
    "${libcarla_source_path}/carla/opendrive/parser/*.h"
    "${libcarla_source_path}/carla/opendrive/parser/pugixml/*.cpp"
    "${libcarla_source_path}/carla/opendrive/parser/pugixml/*.hpp"
    "${libcarla_source_path}/carla/road/*.cpp"
    "${libcarla_source_path}/carla/road/*.h"
    "${libcarla_source_path}/carla/road/element/*.cpp"
    "${libcarla_source_path}/carla/road/element/*.h"
    "${libcarla_source_path}/carla/road/element/cephes/*.cpp"
    "${libcarla_source_path}/carla/road/element/cephes/*.h"
    "${libcarla_source_path}/carla/rpc/*.cpp"
    "${libcarla_source_path}/carla/rpc/*.h"
    "${libcarla_source_path}/carla/sensor/*.h"
    "${libcarla_source_path}/carla/sensor/s11n/*.h"
    "${libcarla_source_path}/carla/sensor/s11n/SensorHeaderSerializer.cpp"
    "${libcarla_source_path}/carla/streaming/*.h"
    "${libcarla_source_path}/carla/streaming/detail/*.cpp"
    "${libcarla_source_path}/carla/streaming/detail/*.h"
    "${libcarla_source_path}/carla/streaming/detail/tcp/*.cpp"
    "${libcarla_source_path}/carla/streaming/detail/tcp/*.h"
    "${libcarla_source_path}/carla/streaming/low_level/*.h")

# Create targets for debug and release in the same build type.
foreach(target carla_server_debug carla_server)
  add_library(${target} STATIC ${libcarla_server_sources})

  target_include_directories(${target} PRIVATE
      "${BOOST_INCLUDE_PATH}"
      "${RPCLIB_INCLUDE_PATH}")

  install(TARGETS ${target} DESTINATION lib)
endforeach(target)

# Specific options for debug.
set_target_properties(carla_server_debug PROPERTIES COMPILE_FLAGS ${CMAKE_CXX_FLAGS_DEBUG})
target_compile_definitions(carla_server_debug PUBLIC -DBOOST_ASIO_ENABLE_BUFFER_DEBUGGING)

# Specific options for release.
set_target_properties(carla_server PROPERTIES COMPILE_FLAGS ${CMAKE_CXX_FLAGS_RELEASE})
