add_subdirectory(api)
add_subdirectory(accumulation)
add_subdirectory(tests)

cc_library(autograd_meta SRCS autograd_meta.cc DEPS pten pten_api)
cc_library(grad_node_info SRCS grad_node_info.cc DEPS pten pten_api)
cc_library(grad_tensor_holder SRCS grad_tensor_holder.cc DEPS grad_node_info gradient_accumulation)
cc_library(utils SRCS utils.cc DEPS pten pten_api global_utils layer proto_desc operator op_registry variable_helper memcpy scale_op autograd_meta)
