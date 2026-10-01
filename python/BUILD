load("@pybind11_bazel//:build_defs.bzl", "pybind_extension")

licenses(["notice"])

config_setting(
    name = "linux",
    constraint_values = ["@platforms//os:linux"],
)

config_setting(
    name = "macos",
    constraint_values = ["@platforms//os:osx"],
)

# Python Binding Extension
pybind_extension(
    name = "visqol_lib_py",
    srcs = ["visqol_lib_py.cc"],
    # Only export the symbol the Python binding needs (PyInit_visqol_lib_py)
    additional_linker_inputs = ["visqol_lib_py.lds"],
    linkopts = select({
        ":linux": ["-Wl,--version-script=$(location visqol_lib_py.lds)"],
        ":macos": ["-Wl,-exported_symbol,_PyInit_visqol_lib_py"],
        "//conditions:default": [],
    }),
    visibility = ["//visibility:public"],
    deps = [
        "//:similarity_result_cc_proto",
        "//:visqol_config_cc_proto",
        "//:visqol_lib",
        "@pybind11_abseil//pybind11_abseil:absl_casters",
        "@pybind11_abseil//pybind11_abseil:status_casters",
        "@pybind11_protobuf//pybind11_protobuf:native_proto_caster",
    ],
)

py_test(
    name = "visqol_lib_py_test",
    srcs = ["visqol_lib_py_test.py"],
    data = [
        ":visqol_lib_py.so",
        "//testdata:clean_speech/CA01_01.wav",
        "//testdata:clean_speech/transcoded_CA01_01.wav",
    ],
    python_version = "PY3",
    deps = [
        "//:similarity_result_py_pb2",
        "//:visqol_config_py_pb2",
        "@com_google_protobuf//:protobuf_python",
    ],
)
