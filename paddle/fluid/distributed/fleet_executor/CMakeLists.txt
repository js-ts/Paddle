proto_library(fleet_executor_desc_proto SRCS fleet_executor_desc.proto)
if(WITH_PYTHON)
  py_proto_compile(fleet_executor_desc_py_proto SRCS fleet_executor_desc.proto)
endif()
proto_library(interceptor_message_proto SRCS interceptor_message.proto)

if(WITH_DISTRIBUTE AND WITH_PSCORE AND NOT (WITH_ASCEND OR WITH_ASCEND_CL))
  set(BRPC_DEPS brpc ssl crypto protobuf gflags glog zlib leveldb snappy gflags glog)
else()
  set(BRPC_DEPS "")
endif()

cc_library(fleet_executor SRCS fleet_executor.cc carrier.cc task_node.cc runtime_graph.cc
        interceptor.cc compute_interceptor.cc interceptor_message_service.cc message_bus.cc
        DEPS proto_desc fleet_executor_desc_proto interceptor_message_proto collective_helper
        ${BRPC_DEPS})

if(WITH_DISTRIBUTE)
  set(DISTRIBUTE_COMPILE_FLAGS "-Wno-non-virtual-dtor -Wno-error=non-virtual-dtor -Wno-error=delete-non-virtual-dtor")
  set_source_files_properties(interceptor.cc PROPERTIES COMPILE_FLAGS ${DISTRIBUTE_COMPILE_FLAGS})
  set_source_files_properties(compute_interceptor.cc PROPERTIES COMPILE_FLAGS ${DISTRIBUTE_COMPILE_FLAGS})
  set_source_files_properties(message_bus.h PROPERTIES COMPILE_FLAGS ${DISTRIBUTE_COMPILE_FLAGS})
  set_source_files_properties(message_bus.cc PROPERTIES COMPILE_FLAGS ${DISTRIBUTE_COMPILE_FLAGS})
  set_source_files_properties(fleet_executor.cc PROPERTIES COMPILE_FLAGS ${DISTRIBUTE_COMPILE_FLAGS})
  set_source_files_properties(carrier.cc PROPERTIES COMPILE_FLAGS ${DISTRIBUTE_COMPILE_FLAGS})
  set_source_files_properties(interceptor_message_service.h PROPERTIES COMPILE_FLAGS ${DISTRIBUTE_COMPILE_FLAGS})
  set_source_files_properties(interceptor_message_service.cc PROPERTIES COMPILE_FLAGS ${DISTRIBUTE_COMPILE_FLAGS})

  add_subdirectory(test)
endif()
