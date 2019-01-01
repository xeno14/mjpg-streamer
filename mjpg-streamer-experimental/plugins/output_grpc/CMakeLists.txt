add_definitions(-D_GNU_SOURCE)

find_package(Protobuf)
find_package(GRPC)

MJPG_STREAMER_PLUGIN_OPTION(output_grpc "gRPC output plugin")

if (PLUGIN_OUTPUT_GRPC)

enable_language(CXX)
  set(PROTOS mjpg_streamer.proto)
  protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS ${PROTOS})
  grpc_generate_cpp(GRPC_SRCS GRPC_HDRS . ${PROTOS})

  MJPG_STREAMER_PLUGIN_COMPILE(output_grpc output_grpc.cpp ${PROTO_SRCS} ${GRPC_SRCS})

  target_link_libraries(output_grpc
    gRPC::grpc++_reflection
    protobuf::libprotobuf
  )
endif()
