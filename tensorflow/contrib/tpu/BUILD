# Description: Operations defined for Cloud TPUs

load(
    "//tensorflow:tensorflow.bzl",
    "tf_custom_op_library",
    "tf_gen_op_libs",
    "tf_gen_op_wrapper_py",
    "tf_py_test",
)
load("//tensorflow:tensorflow.bzl", "tf_custom_op_py_library")

licenses(["notice"])  # Apache 2.0

package(
    default_visibility = [
        "//cloud/vmm/testing/tests/tpu:__subpackages__",
        "//knowledge/cerebra/sense/im2query:__subpackages__",
        "//learning/brain:__subpackages__",
        "//learning/deepmind:__subpackages__",
        "//medical/pathology:__subpackages__",
        "//tensorflow:__subpackages__",
        "//vr/perception:__subpackages__",
    ],
)

py_library(
    name = "tpu_py",
    srcs = ["python/ops/tpu_ops.py"],
    srcs_version = "PY2AND3",
    deps = [
        "//tensorflow/python:framework_for_generated_wrappers",
        "//tensorflow/python:tpu_ops_gen",
    ],
)

py_library(
    name = "async_checkpoint",
    srcs = ["python/tpu/async_checkpoint.py"],
    srcs_version = "PY2AND3",
    deps = [
        "//tensorflow/python:array_ops",
        "//tensorflow/python:control_flow_ops",
        "//tensorflow/python:framework_for_generated_wrappers",
        "//tensorflow/python:init_ops",
        "//tensorflow/python:math_ops",
        "//tensorflow/python:platform",
        "//tensorflow/python:state_ops",
        "//tensorflow/python:summary",
        "//tensorflow/python:summary_ops_v2",
        "//tensorflow/python:training",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python:variables",
        "//tensorflow/python/estimator:estimator_py",
    ],
)

py_library(
    name = "tpu_estimator",
    srcs = [
        "python/tpu/_tpu_estimator_embedding.py",
        "python/tpu/error_handling.py",
        "python/tpu/tpu_config.py",
        "python/tpu/tpu_context.py",
        "python/tpu/tpu_estimator.py",
        "python/tpu/util.py",
    ],
    srcs_version = "PY2AND3",
    deps = [
        ":async_checkpoint",
        ":feature_column",
        ":functional",
        ":tpu_embedding",
        ":tpu_lib",
        "//tensorflow/contrib/training:training_py",
        "//tensorflow/core:protos_all_py",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:control_flow_ops",
        "//tensorflow/python:framework_for_generated_wrappers",
        "//tensorflow/python:function",
        "//tensorflow/python:init_ops",
        "//tensorflow/python:math_ops",
        "//tensorflow/python:platform",
        "//tensorflow/python:session",
        "//tensorflow/python:state_ops",
        "//tensorflow/python:summary",
        "//tensorflow/python:summary_ops_v2",
        "//tensorflow/python:training",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python:variables",
        "//tensorflow/python/estimator:estimator_py",
        "//tensorflow/python/estimator:util",
        "@six_archive//:six",
    ],
)

py_library(
    name = "functional",
    srcs = ["python/tpu/functional.py"],
    srcs_version = "PY2AND3",
    visibility = [
        "//visibility:public",
    ],
    deps = [
        "//tensorflow/python:tpu_ops_gen",
    ],
)

py_library(
    name = "profiler",
    srcs = ["python/profiler/__init__.py"],
    srcs_version = "PY2AND3",
    deps = [
        "//tensorflow/contrib/tpu/profiler:tpu_profiler_analysis_pb2_grpc",
        "//tensorflow/contrib/tpu/profiler:tpu_profiler_analysis_proto_py",
        "//tensorflow/contrib/tpu/profiler:trace_events_proto_py",
        "//tensorflow/python:util",
    ],
)

py_library(
    name = "tpu",
    srcs = [
        "__init__.py",
        "python/tpu/__init__.py",
    ],
    srcs_version = "PY2AND3",
    deps = [
        ":feature_column",
        ":keras_support",  # split out to avoid cycle with tpu_strategy
        ":tpu_embedding",
        ":tpu_estimator",
        ":tpu_lib",
    ],
)

py_library(
    name = "keras_support",
    srcs = [
        "python/tpu/keras_support.py",
        "python/tpu/keras_tpu_variables.py",
    ],
    srcs_version = "PY2AND3",
    visibility = [
        "//cloud/vmm/testing/tests/tpu:__subpackages__",
        "//learning/brain:__subpackages__",
        "//tensorflow:__subpackages__",
        "//third_party/cloud_tpu/models/keras_colab:__subpackages__",
        "//third_party/cloud_tpu/models/resnet50_keras:__subpackages__",
    ],
    deps = [
        ":tpu_lib",
        "//tensorflow/contrib/cluster_resolver:cluster_resolver_py",
        "//tensorflow/contrib/distribute",
        "//tensorflow/contrib/framework:framework_py",
        "//tensorflow/core:protos_all_py",
        "//tensorflow/core/protobuf/tpu:compilation_result_proto_py",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:dtypes",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:linalg_ops",
        "//tensorflow/python:math_ops",
        "//tensorflow/python:platform",
        "//tensorflow/python:random_ops",
        "//tensorflow/python:session",
        "//tensorflow/python:tensor_spec",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/estimator:estimator_py",
        "//tensorflow/python/keras:backend",
        "//tensorflow/python/keras:engine",
        "//tensorflow/python/keras:layers",
        "//third_party/py/numpy",
    ],
)

py_library(
    name = "tpu_lib",
    srcs = [
        "python/tpu/__init__.py",
        "python/tpu/bfloat16.py",
        "python/tpu/device_assignment.py",
        "python/tpu/session_support.py",
        "python/tpu/tensor_tracer.py",
        "python/tpu/topology.py",
        "python/tpu/tpu.py",
        "python/tpu/tpu_feed.py",
        "python/tpu/tpu_function.py",
        "python/tpu/tpu_optimizer.py",
        "python/tpu/tpu_sharding.py",
        "python/tpu/tpu_system_metadata.py",
        "python/tpu/training_loop.py",
    ],
    srcs_version = "PY2AND3",
    deps = [
        ":datasets",
        ":functional",
        ":profiler",
        ":tpu_py",
        "//tensorflow/compiler/xla/experimental/xla_sharding",
        "//tensorflow/compiler/xla/python_api:xla_shape",
        "//tensorflow/contrib/cluster_resolver:cluster_resolver_py",
        "//tensorflow/contrib/compiler:xla",
        "//tensorflow/core:protos_all_py",
        "//tensorflow/core/protobuf/tpu:compilation_result_proto_py",
        "//tensorflow/core/protobuf/tpu:dynamic_padding_proto_py",
        "//tensorflow/core/protobuf/tpu:optimization_parameters_proto_py",
        "//tensorflow/core/protobuf/tpu:topology_proto_py",
        "//tensorflow/core/protobuf/tpu:tpu_embedding_configuration_proto_py",
        "//tensorflow/core/protobuf/tpu:tpu_embedding_output_layout_proto_py",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:control_flow_ops",
        "//tensorflow/python:control_flow_util",
        "//tensorflow/python:dtypes",
        "//tensorflow/python:framework",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:tensor_shape",
        "//tensorflow/python:tpu_ops_gen",
        "//tensorflow/python:training",
        "//tensorflow/python:util",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python/ops/losses",
    ],
)

py_library(
    name = "datasets",
    srcs = [
        "python/tpu/datasets.py",
    ],
    srcs_version = "PY2AND3",
    deps = [
        "//tensorflow/contrib/data/python/ops:batching",
        "//tensorflow/contrib/data/python/ops:interleave_ops",
        "//tensorflow/python:dtypes",
        "//tensorflow/python:function",
        "//tensorflow/python:functional_ops",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/data/ops:iterator_ops",
        "//tensorflow/python/data/ops:readers",
    ],
)

tf_py_test(
    name = "datasets_test",
    size = "medium",
    srcs = ["python/tpu/datasets_test.py"],
    additional_deps = [
        "//tensorflow/python:client_testlib",
        ":datasets",
    ],
    grpc_enabled = True,
    shard_count = 4,
    tags = ["no_oss"],
)

tf_py_test(
    name = "tpu_test",
    size = "small",
    srcs = ["python/tpu/tpu_test.py"],
    additional_deps = [
        ":tpu",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:dtypes",
        "//tensorflow/python:framework",
        "//tensorflow/python:layers",
    ],
    tags = ["no_windows"],  # TODO: needs investigation on Windows
)

tf_py_test(
    name = "tpu_sharding_test",
    size = "small",
    srcs = ["python/tpu/tpu_sharding_test.py"],
    additional_deps = [
        ":tpu",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:framework",
    ],
)

tf_py_test(
    name = "bfloat16_test",
    size = "small",
    srcs = ["python/tpu/bfloat16_test.py"],
    additional_deps = [
        ":tpu",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:framework",
    ],
)

tf_py_test(
    name = "tpu_infeed_test",
    size = "small",
    srcs = ["python/tpu/tpu_infeed_test.py"],
    additional_deps = [
        ":tpu",
        "//tensorflow/python:framework",
        "//tensorflow/python:framework_test_lib",
    ],
)

tf_py_test(
    name = "tpu_config_test",
    size = "small",
    srcs = ["python/tpu/tpu_config_test.py"],
    additional_deps = [
        ":tpu_estimator",
        "//tensorflow/python:framework",
        "//tensorflow/python:framework_test_lib",
    ],
)

tf_py_test(
    name = "tpu_estimator_signals_test",
    size = "small",
    srcs = ["python/tpu/tpu_estimator_signals_test.py"],
    additional_deps = [
        ":tpu_estimator",
        "//tensorflow/python:framework",
        "//tensorflow/python:framework_test_lib",
    ],
)

tf_py_test(
    name = "topology_test",
    size = "medium",
    srcs = ["python/tpu/topology_test.py"],
    additional_deps = [
        ":tpu",
        "//tensorflow/python:framework_test_lib",
    ],
)

py_library(
    name = "tpu_embedding",
    srcs = [
        "python/tpu/tpu_embedding.py",
        "python/tpu/tpu_embedding_gradient.py",
    ],
    srcs_version = "PY2AND3",
    deps = [
        ":tpu_lib",
        "//tensorflow/core/protobuf/tpu:tpu_embedding_configuration_proto_py",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:framework_for_generated_wrappers",
        "//tensorflow/python:init_ops",
        "//tensorflow/python:math_ops",
        "//tensorflow/python:partitioned_variables",
        "//tensorflow/python:tpu_ops_gen",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python:variables",
        "@six_archive//:six",
    ],
)

py_library(
    name = "feature_column",
    srcs = ["python/tpu/feature_column.py"],
    deps = [
        ":tpu_lib",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:init_ops",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python/feature_column",
        "//tensorflow/python/feature_column:feature_column_py",
    ],
)

tf_py_test(
    name = "feature_column_test",
    srcs = [
        "python/tpu/feature_column_test.py",
    ],
    additional_deps = [
        ":feature_column",
        "//third_party/py/numpy",
        "//tensorflow/python:dtypes",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:lookup_ops",
        "//tensorflow/python:parsing_ops",
        "//tensorflow/python:session",
        "//tensorflow/python:sparse_tensor",
        "//tensorflow/python:variables",
        "//tensorflow/python/feature_column",
        "//tensorflow/python/feature_column:feature_column_py",
    ],
    main = "python/tpu/feature_column_test.py",
)
