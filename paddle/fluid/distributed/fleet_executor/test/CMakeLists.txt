set_source_files_properties(interceptor_ping_pong_test.cc PROPERTIES COMPILE_FLAGS ${DISTRIBUTE_COMPILE_FLAGS})
set_source_files_properties(compute_interceptor_test.cc PROPERTIES COMPILE_FLAGS ${DISTRIBUTE_COMPILE_FLAGS})
cc_test(interceptor_ping_pong_test SRCS interceptor_ping_pong_test.cc DEPS fleet_executor ${BRPC_DEPS})
cc_test(compute_interceptor_test SRCS compute_interceptor_test.cc DEPS fleet_executor ${BRPC_DEPS})
if(WITH_DISTRIBUTE AND WITH_PSCORE AND NOT (WITH_ASCEND OR WITH_ASCEND_CL))
set_source_files_properties(interceptor_ping_pong_with_brpc_test.cc PROPERTIES COMPILE_FLAGS ${DISTRIBUTE_COMPILE_FLAGS})
cc_test(interceptor_ping_pong_with_brpc_test SRCS interceptor_ping_pong_with_brpc_test.cc DEPS fleet_executor ${BRPC_DEPS})
endif()
