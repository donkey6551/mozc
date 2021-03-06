# -*- coding: utf-8 -*-
# Copyright 2010-2020, Google Inc.
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are
# met:
#
#     * Redistributions of source code must retain the above copyright
# notice, this list of conditions and the following disclaimer.
#     * Redistributions in binary form must reproduce the above
# copyright notice, this list of conditions and the following disclaimer
# in the documentation and/or other materials provided with the
# distribution.
#     * Neither the name of Google Inc. nor the names of its
# contributors may be used to endorse or promote products derived from
# this software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
# A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
# OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
# SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
# LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
# DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
# THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

load(
    "//:build_defs.bzl",
    "cc_binary_mozc",
    "cc_library_mozc",
    "cc_test_mozc",
    "objc_library_mozc",
    "py_binary_mozc",
    "py_library_mozc",
    "select_mozc",
)

package(default_visibility = [
    "//:__subpackages__",
])

exports_files([
    "client.h",
    "util.h",
])

test_suite(
    name = "nacl_test",
    tests = [
        ":bitarray_test",
        ":clock_mock_test",
        ":clock_test",
        ":cpu_stats_test",
        ":embedded_file_test",
        ":flags_test",
        ":hash_test",
        ":iterator_adapter_test",
        ":logging_test",
        ":mutex_test",
        ":number_util_test",
        ":obfuscator_support_test",
        ":scheduler_stub_test",
        ":scheduler_test",
        ":serialized_string_array_test",
        ":singleton_test",
        ":stl_util_test",
        ":stopwatch_test",
        ":system_util_test",
        ":text_normalizer_test",
        ":thread_test",
        ":trie_test",
        ":unnamed_event_test",
        ":url_test",
        ":util_test",
        ":version_test",
        # Disabled tests due to file IO error.
        # ":config_file_stream_test",
        # ":encryptor_test",
        # ":file_util_test",
        # ":mmap_test",
        # ":multifile_test",
        # ":process_mutex_test",
    ],
)

# Test suite needs to have exact test names,
# because android tests are tagged "manual".
test_suite(
    name = "android_test",
    tests = [
        ":bitarray_test_android",
        ":clock_mock_test_android",
        ":clock_test_android",
        ":config_file_stream_test_android",
        ":cpu_stats_test_android",
        ":encryptor_test_android",
        ":flags_test_android",
        ":hash_test_android",
        ":iterator_adapter_test_android",
        ":logging_test_android",
        ":mmap_test_android",
        ":mutex_test_android",
        ":number_util_test_android",
        ":obfuscator_support_test_android",
        ":process_mutex_test_android",
        ":scheduler_stub_test_android",
        ":scheduler_test_android",
        ":serialized_string_array_test_android",
        ":singleton_test_android",
        ":stl_util_test_android",
        ":stopwatch_test_android",
        ":system_util_test_android",
        ":text_normalizer_test_android",
        ":thread_test_android",
        ":trie_test_android",
        ":unnamed_event_test_android",
        ":url_test_android",
        ":version_test_android",
        # Disabled tests due to errors.
        # ":embedded_file_test_android",
        # ":file_util_test_android",
        # ":multifile_test_android",
        # ":util_test_android",
    ],
)

cc_library_mozc(
    name = "base",
    hdrs = [
        "compiler_specific.h",
        "const.h",
    ],
    deps = [
        ":compiler_specific",
        ":const",
        ":file_stream",
        ":file_util",
        ":flags",
        ":mmap",
        ":mutex",
        ":number_util",
        ":port",
        ":process_mutex",
        ":singleton",
        ":system_util",
        ":thread",
        ":util",
    ],
)

cc_library_mozc(
    name = "compiler_specific",
    hdrs = ["compiler_specific.h"],
    visibility = ["//visibility:private"],
)

cc_library_mozc(
    name = "flags",
    srcs = ["flags.cc"],
    hdrs = ["flags.h"],
    deps = [
        ":port",
        "@com_google_absl//absl/flags:flag",
    ] + select_mozc(
        client = [":singleton"],
        default = ["//base"],
    ),
)

cc_test_mozc(
    name = "flags_test",
    size = "small",
    srcs = ["flags_test.cc"],
    deps = [
        ":flags",
        "//testing:gunit_main",
    ],
)

INIT_MOZC_BUILDTOOL_DEPS = [
    ":file_util",
    ":flags",
    ":logging",
]

INIT_MOZC_APP_BUILD_DEPS = INIT_MOZC_BUILDTOOL_DEPS + [
    ":system_util",
]

cc_library_mozc(
    name = "init_mozc_buildtool",
    srcs = ["init_mozc.cc"],
    hdrs = ["init_mozc.h"],
    copts = ["-DMOZC_BUILDTOOL_BUILD"],
    visibility = [
        "//:__subpackages__",
    ],
    deps = select_mozc(
        client = INIT_MOZC_BUILDTOOL_DEPS,
        default = ["//base"],
    ) + [":system_util"],
)

cc_library_mozc(
    name = "init_mozc",
    srcs = ["init_mozc.cc"],
    hdrs = ["init_mozc.h"],
    visibility = [
        "//:__subpackages__",
    ],
    deps = select_mozc(
        client = INIT_MOZC_APP_BUILD_DEPS,
        default = ["//base"],
    ),
)

cc_library_mozc(
    name = "nacl_linkopts",
    linkopts = select({
        "//tools/cc_target_os:nacl": [
            "-lppapi",
            "-lppapi_cpp",
        ],
        "//conditions:default": [],
    }),
    visibility = ["//:__subpackages__"],
)

cc_library_mozc(
    name = "port",
    hdrs = ["port.h"],
    deps = select_mozc(
        default = ["//base:base"],
        linux = ["@com_google_absl//absl/base:core_headers"],
        nacl = [
            # TODO(hsumita): Move :nacl_linkopts to more suitable position.
            ":nacl_linkopts",
            "//base:base",
        ],
        oss = ["@com_google_absl//absl/base:core_headers"],
    ),
)

cc_library_mozc(
    name = "const",
    hdrs = ["const.h"],
    visibility = ["//visibility:private"],
)

cc_library_mozc(
    name = "scoped_handle",
    srcs = ["scoped_handle.cc"],
    hdrs = ["scoped_handle.h"],
    visibility = ["//:__subpackages__"],
)

cc_library_mozc(
    name = "freelist",
    hdrs = ["freelist.h"],
    visibility = ["//:__subpackages__"],
    deps = [":port"],
)

LOGGING_APP_BUILD_DEPS = [
    ":const",
    ":clock",
    ":flags",
    ":mutex",
    ":singleton",
]

cc_library_mozc(
    name = "logging",
    srcs = ["logging.cc"],
    hdrs = ["logging.h"],
    linkopts = select_mozc(
        # Android requires linking logging library.
        android = ["-llog"],
    ),
    deps = select_mozc(
        client = LOGGING_APP_BUILD_DEPS,
        default = ["//base"],
    ) + [":port"],
)

cc_test_mozc(
    name = "logging_test",
    size = "small",
    srcs = ["logging_test.cc"],
    deps = [
        ":logging",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "codegen_bytearray_stream",
    hdrs = ["codegen_bytearray_stream.h"],
    visibility = [
        "//:__subpackages__",
    ],
    deps = [
        ":port",
    ],
)

cc_test_mozc(
    name = "codegen_bytearray_stream_test",
    size = "small",
    srcs = ["codegen_bytearray_stream_test.cc"],
    tags = ["no_android"],
    visibility = ["//visibility:private"],
    deps = [
        ":codegen_bytearray_stream",
        ":port",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "unnamed_event",
    srcs = ["unnamed_event.cc"],
    hdrs = ["unnamed_event.h"],
    visibility = ["//:__subpackages__"],
    deps = [
        ":logging",
        ":port",
        ":scoped_handle",
    ],
)

cc_test_mozc(
    name = "unnamed_event_test",
    size = "small",
    srcs = ["unnamed_event_test.cc"],
    deps = [
        ":logging",
        ":port",
        ":thread",
        ":unnamed_event",
        ":util",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "thread",
    srcs = ["thread.cc"],
    hdrs = ["thread.h"],
    visibility = [
        "//:__subpackages__",
    ],
    deps = [
        ":logging",
        ":port",
    ],
)

cc_test_mozc(
    name = "thread_test",
    size = "small",
    srcs = [
        "thread_test.cc",
    ],
    deps = [
        ":thread",
        ":util",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "win_util",
    hdrs = ["win_util.h"],
    visibility = ["//:__subpackages__"],
    deps = [
        ":port",
        "//testing:gunit_prod",
    ],
)

cc_library_mozc(
    name = "stl_util",
    hdrs = ["stl_util.h"],
    visibility = ["//:__subpackages__"],
    deps = [":port"],
)

cc_test_mozc(
    name = "stl_util_test",
    size = "small",
    srcs = ["stl_util_test.cc"],
    deps = [
        ":stl_util",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "iterator_adapter",
    hdrs = ["iterator_adapter.h"],
    visibility = [
        "//:__subpackages__",
    ],
)

cc_test_mozc(
    name = "iterator_adapter_test",
    size = "small",
    srcs = ["iterator_adapter_test.cc"],
    deps = [
        ":iterator_adapter",
        ":port",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "singleton",
    srcs = ["singleton.cc"],
    hdrs = ["singleton.h"],
    visibility = ["//:__subpackages__"],
    deps = [":mutex"],
)

cc_test_mozc(
    name = "singleton_test",
    size = "small",
    srcs = ["singleton_test.cc"],
    deps = [
        ":singleton",
        ":thread",
        ":util",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "mozc_hash_map",
    hdrs = ["mozc_hash_map.h"],
    visibility = [
        "//:__subpackages__",
    ],
    deps = select_mozc(
        default = ["@com_google_absl//absl/container:flat_hash_map"],
        linux = [],
        oss = [],
    ),
)

cc_library_mozc(
    name = "mozc_hash_set",
    hdrs = ["mozc_hash_set.h"],
    visibility = ["//:__subpackages__"],
    deps = select_mozc(
        default = ["@com_google_absl//absl/container:flat_hash_set"],
        linux = [],
        oss = [],
    ),
)

genrule(
    name = "character_set_data",
    srcs = [
        "gen_character_set.py",
        "//data/unicode:CP932.TXT",
        "//data/unicode:JIS0201.TXT",
        "//data/unicode:JIS0208.TXT",
        "//data/unicode:JIS0212.TXT",
        "//data/unicode:jisx0213-2004-std.txt",
    ],
    outs = ["character_set.inc"],
    cmd = "python2.7 $(location :gen_character_set.py) --cp932file=$(location //data/unicode:CP932.TXT) --jisx0201file=$(location //data/unicode:JIS0201.TXT) --jisx0208file=$(location //data/unicode:JIS0208.TXT) --jisx0212file=$(location //data/unicode:JIS0212.TXT) --jisx0213file=$(location //data/unicode:jisx0213-2004-std.txt) --output=$@",
    local = 0,
    visibility = ["//visibility:private"],
)

cc_library_mozc(
    name = "double_array_def",
    hdrs = ["double_array.h"],
    deps = [
        ":port",
    ],
)

cc_library_mozc(
    name = "japanese_util_rule",
    srcs = ["japanese_util_rule.cc"],
    hdrs = ["japanese_util_rule.h"],
    visibility = ["//visibility:private"],
    deps = [":double_array_def"],
)

cc_library_mozc(
    name = "hash",
    srcs = ["hash.cc"],
    hdrs = ["hash.h"],
    deps = [
        ":port",
        "@com_google_absl//absl/strings",
    ],
)

cc_test_mozc(
    name = "hash_test",
    srcs = ["hash_test.cc"],
    deps = [
        ":hash",
        ":port",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "clock",
    srcs = ["clock.cc"],
    hdrs = ["clock.h"],
    deps = [
        ":port",
        ":singleton",
    ],
)

cc_test_mozc(
    name = "clock_test",
    srcs = ["clock_test.cc"],
    deps = [
        ":clock",
        ":clock_mock",
        ":mutex",
        ":port",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "util",
    srcs = [
        "util.cc",
        ":character_set_data",
    ],
    hdrs = ["util.h"],
    visibility = [
        "//:__subpackages__",
    ],
    deps = [
        ":double_array_def",
        ":japanese_util_rule",
        ":logging",
        ":port",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
    ],
)

cc_test_mozc(
    name = "util_test",
    size = "small",
    srcs = [
        "util_test.cc",
    ],
    data = ["//data/test/character_set:character_set.tsv"],
    deps = [
        ":compiler_specific",
        ":file_stream",
        ":file_util",
        ":logging",
        ":number_util",
        ":port",
        ":util",
        "//testing:gunit_main",
        "//testing:mozctest",
    ],
)

cc_library_mozc(
    name = "mutex",
    srcs = ["mutex.cc"],
    hdrs = ["mutex.h"],
    linkopts = select_mozc(
        linux = ["-pthread"],
    ),
    visibility = [
        "//:__subpackages__",
    ],
    deps = [
        ":port",
        ":win_util",
        "@com_google_absl//absl/base:core_headers",
    ],
)

cc_test_mozc(
    name = "mutex_test",
    size = "small",
    srcs = [
        "mutex_test.cc",
    ],
    deps = [
        ":clock",
        ":logging",
        ":mutex",
        ":thread",
        ":util",
        "//testing:gunit_main",
        "@com_google_absl//absl/base:core_headers",
    ],
)

cc_library_mozc(
    name = "file_stream",
    srcs = ["file_stream.cc"],
    hdrs = ["file_stream.h"],
    visibility = [
        "//:__subpackages__",
    ],
)

cc_binary_mozc(
    name = "text_converter_compiler",
    srcs = ["text_converter_compiler.cc"],
    deps = [
        ":double_array_def",
        ":file_stream",
        ":flags",
        ":init_mozc_buildtool",
        ":logging",
        ":util",
        "//third_party/darts/v0_32",
    ],
)

cc_library_mozc(
    name = "number_util",
    srcs = ["number_util.cc"],
    hdrs = ["number_util.h"],
    visibility = [
        "//:__subpackages__",
    ],
    deps = [
        ":double_array_def",
        ":japanese_util_rule",
        ":logging",
        ":port",
        ":util",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
    ],
)

cc_test_mozc(
    name = "number_util_test",
    size = "small",
    srcs = ["number_util_test.cc"],
    deps = [
        ":number_util",
        ":port",
        "//testing:googletest",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "trie",
    hdrs = ["trie.h"],
    visibility = ["//:__subpackages__"],
    deps = [
        ":logging",
        ":util",
        "@com_google_absl//absl/strings",
    ],
)

cc_test_mozc(
    name = "trie_test",
    size = "small",
    srcs = ["trie_test.cc"],
    deps = [
        ":trie",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "text_normalizer",
    srcs = ["text_normalizer.cc"],
    hdrs = ["text_normalizer.h"],
    visibility = ["//:__subpackages__"],
    deps = [
        ":port",
        ":util",
        "@com_google_absl//absl/strings",
    ],
)

cc_test_mozc(
    name = "text_normalizer_test",
    size = "small",
    srcs = ["text_normalizer_test.cc"],
    deps = [
        ":text_normalizer",
        ":util",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "clock_mock",
    testonly = 1,
    srcs = ["clock_mock.cc"],
    hdrs = [
        "clock.h",  # For ClockInterface
        "clock_mock.h",
    ],
    visibility = ["//:__subpackages__"],
    deps = [
        ":port",
        ":util",
    ],
)

cc_test_mozc(
    name = "clock_mock_test",
    size = "small",
    srcs = ["clock_mock_test.cc"],
    deps = [
        ":clock_mock",
        ":util",
        "//testing:googletest",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "bitarray",
    hdrs = ["bitarray.h"],
    visibility = ["//:__subpackages__"],
    deps = [":port"],
)

cc_test_mozc(
    name = "bitarray_test",
    size = "small",
    srcs = ["bitarray_test.cc"],
    deps = [
        ":bitarray",
        ":util",
        "//testing:googletest",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "mmap_sync_interface",
    hdrs = ["mmap_sync_interface.h"],
    visibility = ["//visibility:private"],
)

cc_library_mozc(
    name = "mmap",
    srcs = ["mmap.cc"],
    hdrs = ["mmap.h"],
    deps = [
        ":logging",
        ":mmap_sync_interface",
        ":port",
        ":scoped_handle",
        ":util",
    ] + select(
        {
            "//tools/cc_target_os:nacl": [
                ":mutex",
                ":file_stream",
                "@com_google_absl//absl/base",
            ],
            "//conditions:default": [],
        },
    ),
)

cc_test_mozc(
    name = "mmap_test",
    size = "small",
    srcs = ["mmap_test.cc"],
    deps = [
        ":file_stream",
        ":file_util",
        ":flags",
        ":mmap",
        ":util",
        "//testing:googletest",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "file_util",
    srcs = ["file_util.cc"],
    hdrs = ["file_util.h"],
    visibility = ["//:__subpackages__"],
    deps = [
        ":file_stream",
        ":logging",
        ":mmap",
        ":mutex",
        ":port",
        ":scoped_handle",
        ":util",
        ":win_util",
        "@com_google_absl//absl/strings",
    ],
)

cc_test_mozc(
    name = "file_util_test",
    size = "small",
    srcs = ["file_util_test.cc"],
    deps = [
        ":file_stream",
        ":file_util",
        ":logging",
        ":number_util",
        ":util",
        "//testing:googletest",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "system_util",
    srcs = select_mozc(
        android = [
            "android_util.cc",
        ],
    ) + ["system_util.cc"],
    hdrs = select_mozc(
        android = [
            "android_util.h",
        ],
    ) + ["system_util.h"],
    visibility = [
        "//:__subpackages__",
    ],
    deps = select_mozc(
        android = [
            # android_util.h includes this for FRIEND_TEST
            "//testing:gunit_prod",
        ],
        ios = [":mac_util"],
    ) + [
        ":const",
        ":file_util",
        ":logging",
        ":number_util",
        ":port",
        ":singleton",
        ":util",
    ] + [
        ":mutex",
        ":win_util",
    ],
)

cc_test_mozc(
    name = "system_util_test",
    srcs = ["system_util_test.cc"],
    visibility = ["//visibility:private"],
    deps = [
        ":number_util",
        ":port",
        ":system_util",
        ":util",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "process",
    srcs = ["process.cc"],
    hdrs = ["process.h"],
    visibility = ["//:__subpackages__"],
    deps = [
        ":const",
        ":file_util",
        ":logging",
        ":port",
        ":scoped_handle",
        ":system_util",
        ":util",
        ":win_util",
    ],
)

cc_binary_mozc(
    name = "process_main",
    srcs = ["process_main.cc"],
    deps = [
        ":flags",
        ":init_mozc",
        ":logging",
        ":process",
    ],
)

cc_library_mozc(
    name = "process_mutex",
    srcs = ["process_mutex.cc"],
    hdrs = ["process_mutex.h"],
    visibility = ["//:__subpackages__"],
    deps = [
        ":file_util",
        ":logging",
        ":mutex",
        ":port",
        ":scoped_handle",
        ":singleton",
        ":system_util",
        ":util",
    ],
)

cc_test_mozc(
    name = "process_mutex_test",
    size = "small",
    srcs = ["process_mutex_test.cc"],
    deps = [
        "logging",
        ":file_util",
        ":process_mutex",
        ":system_util",
        ":util",
        "//testing:googletest",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "run_level",
    srcs = [
        "run_level.cc",
        "win_sandbox.h",
    ],
    hdrs = ["run_level.h"],
    visibility = ["//:__subpackages__"],
    deps = [
        ":const",
        ":logging",
        ":port",
        ":scoped_handle",
        ":system_util",
        ":util",
        ":win_util",
    ],
)

cc_binary_mozc(
    name = "run_level_main",
    srcs = ["run_level_main.cc"],
    deps = [
        ":flags",
        ":init_mozc",
        ":run_level",
    ],
)

py_library_mozc(
    name = "gen_config_file_stream_data_lib",
    srcs = ["gen_config_file_stream_data.py"],
)

py_binary_mozc(
    name = "gen_config_file_stream_data",
    srcs = ["gen_config_file_stream_data.py"],
    deps = [":gen_config_file_stream_data_lib"],
)

genrule(
    name = "config_file_stream_data",
    srcs = [
        "//data/keymap:atok.tsv",
        "//data/keymap:chromeos.tsv",
        "//data/keymap:kotoeri.tsv",
        "//data/keymap:mobile.tsv",
        "//data/keymap:ms-ime.tsv",
        "//data/preedit:composition_table",
    ],
    outs = ["config_file_stream_data.inc"],
    cmd = "$(location :gen_config_file_stream_data) --output=$@ $(SRCS)",
    exec_tools = [":gen_config_file_stream_data"],
    visibility = ["//visibility:private"],
)

cc_library_mozc(
    name = "config_file_stream",
    srcs = [
        "config_file_stream.cc",
        ":config_file_stream_data",
    ],
    hdrs = ["config_file_stream.h"],
    visibility = ["//:__subpackages__"],
    deps = [
        ":file_stream",
        ":file_util",
        ":logging",
        ":port",
        ":singleton",
        ":system_util",
        ":util",
    ],
)

cc_test_mozc(
    name = "config_file_stream_test",
    size = "small",
    srcs = ["config_file_stream_test.cc"],
    deps = [
        ":config_file_stream",
        ":file_util",
        ":system_util",
        "//testing:googletest",
        "//testing:gunit_main",
    ],
)

TARGET_PLATFORM = select({
    "//tools/cc_target_os:oss_android": "Android",
    "//tools/cc_target_os:android": "Android",
    "//tools/cc_target_os:nacl": "NaCl",
    "//tools/cc_target_os:wasm": "Wasm",
    "//tools/cc_target_os:linux": "Linux",
    "//tools/cc_target_os:oss_linux": "Linux",
})

genrule(
    name = "mozc_version_txt",
    srcs = [
        "//data/version:mozc_version_template.bzl",
    ],
    outs = ["mozc_version.txt"],
    cmd = ("$(location //build_tools:mozc_version)" +
           ' --template_path="$(location //data/version:mozc_version_template.bzl)"' +
           ' --output="$(OUTS)"' +
           " --target_platform=" + TARGET_PLATFORM),
    tools = ["//build_tools:mozc_version"],
    visibility = ["//visibility:private"],
)

genrule(
    name = "version_def_h",
    srcs = [
        "mozc_version.txt",
        "version_def_template.h",
    ],
    outs = ["version_def.h"],
    cmd = "$(location //build_tools:replace_version) --version_file=$(location mozc_version.txt) --input=$(location version_def_template.h) --output=$(OUTS) --branding=GoogleJapaneseInput",
    tools = ["//build_tools:replace_version"],
    visibility = ["//visibility:private"],
)

cc_library_mozc(
    name = "version",
    srcs = [
        "version.cc",
        ":version_def_h",
    ],
    hdrs = [
        "version.h",
        "version_def.h",
    ],
    deps = [
        ":logging",
        ":number_util",
        ":port",
        ":util",
    ],
)

cc_test_mozc(
    name = "version_test",
    size = "small",
    srcs = ["version_test.cc"],
    deps = [
        ":util",
        ":version",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "obfuscator_support",
    srcs = [
        "unverified_aes256.cc",
        "unverified_sha1.cc",
    ],
    hdrs = [
        "unverified_aes256.h",
        "unverified_sha1.h",
    ],
    visibility = [
        "//data_manager:__pkg__",
        "//ipc:__pkg__",
    ],
    deps = [
        ":logging",
        ":port",
        "@com_google_absl//absl/strings",
    ],
)

cc_test_mozc(
    name = "obfuscator_support_test",
    size = "small",
    srcs = [
        "unverified_aes256_test.cc",
        "unverified_sha1_test.cc",
    ],
    visibility = ["//visibility:private"],
    deps = [
        ":obfuscator_support",
        "//testing:googletest",
        "//testing:gunit_main",
    ],
)

# TODO(team): encryptor.cc and password_manager.cc are mutually dependent and
# cannot be decoupled.  Fix this issue.
cc_library_mozc(
    name = "encryptor",
    srcs = [
        "encryptor.cc",
        "password_manager.cc",
    ],
    hdrs = [
        "encryptor.h",
        "password_manager.h",
    ],
    visibility = ["//:__subpackages__"],
    deps = [
        ":const",
        ":file_stream",
        ":file_util",
        ":logging",
        ":mmap",
        ":mutex",
        ":obfuscator_support",
        ":port",
        ":singleton",
        ":system_util",
        ":util",
    ] + select_mozc(
        ios = [":mac_util"],
    ),
)

cc_test_mozc(
    name = "encryptor_test",
    size = "small",
    srcs = [
        "encryptor_test.cc",
        "password_manager_test.cc",
    ],
    visibility = ["//visibility:private"],
    deps = [
        ":encryptor",
        ":system_util",
        ":util",
        "//testing:googletest",
        "//testing:gunit_main",
    ],
)

cc_binary_mozc(
    name = "encryptor_main",
    srcs = ["encryptor_main.cc"],
    visibility = ["//visibility:private"],
    deps = [
        ":encryptor",
        ":file_stream",
        ":flags",
        ":init_mozc",
        ":logging",
        ":mmap",
        ":util",
    ],
)

cc_binary_mozc(
    name = "password_manager_main",
    srcs = ["password_manager_main.cc"],
    visibility = ["//visibility:private"],
    deps = [
        ":encryptor",
        ":flags",
        ":init_mozc",
        ":logging",
        ":util",
    ],
)

cc_library_mozc(
    name = "scheduler",
    srcs = [
        "scheduler.cc",
    ],
    hdrs = ["scheduler.h"],
    deps = [
        ":clock",
        ":logging",
        ":mutex",
        ":port",
        ":singleton",
        ":thread",
        ":unnamed_event",
        ":util",
    ],
)

cc_test_mozc(
    name = "scheduler_test",
    size = "small",
    srcs = ["scheduler_test.cc"],
    deps = [
        ":logging",
        ":scheduler",
        ":unnamed_event",
        ":util",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "scheduler_stub",
    testonly = 1,
    srcs = ["scheduler_stub.cc"],
    hdrs = ["scheduler_stub.h"],
    visibility = ["//:__subpackages__"],
    deps = [
        ":logging",
        ":port",
        ":scheduler",
    ],
)

cc_test_mozc(
    name = "scheduler_stub_test",
    size = "small",
    srcs = ["scheduler_stub_test.cc"],
    deps = [
        ":scheduler_stub",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "cpu_stats",
    srcs = ["cpu_stats.cc"],
    hdrs = ["cpu_stats.h"],
    visibility = ["//:__subpackages__"],
    deps = [
        ":logging",
        ":port",
    ],
)

cc_test_mozc(
    name = "cpu_stats_test",
    size = "small",
    srcs = ["cpu_stats_test.cc"],
    deps = [
        ":cpu_stats",
        "//testing:googletest",
        "//testing:gunit_main",
    ],
)

cc_binary_mozc(
    name = "cpu_stats_main",
    srcs = ["cpu_stats_main.cc"],
    deps = [
        ":cpu_stats",
        ":flags",
        ":init_mozc",
        ":port",
        ":thread",
        ":util",
    ],
)

cc_library_mozc(
    name = "stopwatch",
    srcs = ["stopwatch.cc"],
    hdrs = ["stopwatch.h"],
    visibility = ["//:__subpackages__"],
    deps = [
        ":clock",
        ":port",
    ],
)

cc_test_mozc(
    name = "stopwatch_test",
    size = "small",
    srcs = ["stopwatch_test.cc"],
    deps = [
        ":clock",
        ":clock_mock",
        ":stopwatch",
        "//testing:gunit_main",
    ],
)

cc_binary_mozc(
    name = "stopwatch_main",
    srcs = ["stopwatch_main.cc"],
    deps = [
        ":flags",
        ":init_mozc",
        ":stopwatch",
        ":util",
    ],
)

cc_library_mozc(
    name = "url",
    srcs = ["url.cc"],
    hdrs = ["url.h"],
    visibility = ["//:__subpackages__"],
    deps = [
        ":logging",
        ":port",
        ":singleton",
        ":util",
        ":version",
    ],
)

cc_test_mozc(
    name = "url_test",
    size = "small",
    srcs = ["url_test.cc"],
    deps = [
        ":url",
        ":util",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "multifile",
    srcs = ["multifile.cc"],
    hdrs = ["multifile.h"],
    visibility = [
        "//:__subpackages__",
    ],
    deps = [
        ":file_stream",
        ":logging",
        ":port",
        ":util",
    ],
)

cc_test_mozc(
    name = "multifile_test",
    size = "small",
    srcs = ["multifile_test.cc"],
    deps = [
        ":file_stream",
        ":file_util",
        ":flags",
        ":multifile",
        ":util",
        "//testing:googletest",
        "//testing:gunit_main",
    ],
)

cc_library_mozc(
    name = "coordinates",
    hdrs = ["coordinates.h"],
    visibility = ["//:__subpackages__"],
)

cc_library_mozc(
    name = "crash_report_handler",
    srcs = ["crash_report_handler.cc"],
    hdrs = ["crash_report_handler.h"],
    deps = [
        ":base",
        ":file_util",
        ":logging",
        ":system_util",
        ":util",
        ":version",
    ],
)

objc_library_mozc(
    name = "mac_util",
    srcs = [
        "mac_util.mm",
        "scoped_cftyperef.h",
    ],
    hdrs = ["mac_util.h"],
    visibility = ["//:__subpackages__"],
    deps = [
        ":const",
        ":logging",
        ":port",
        ":singleton",
        ":util",
    ],
)

cc_library_mozc(
    name = "winmain",
    hdrs = ["winmain.h"],
    visibility = [
        # For //server:mozc_server.
        "//server:__pkg__",
    ],
    deps = [
        ":base",
        ":port",
        ":util",
    ],
)

cc_library_mozc(
    name = "win_api_test_helper",
    hdrs = ["win_api_test_helper.h"],
    visibility = [
        # For //config:stats_config_util_test.
        "//config:__pkg__",
    ],
    deps = [":port"],
)

cc_library_mozc(
    name = "embedded_file",
    hdrs = ["embedded_file.h"],
    visibility = ["//:__subpackages__"],
    deps = [
        ":port",
        "@com_google_absl//absl/strings",
    ],
)

genrule(
    name = "gen_embedded_file_test_data",
    srcs = ["embedded_file.h"],
    outs = ["embedded_file_test_data.inc"],
    cmd = "$(location //build_tools:embed_file) --input=$< --name=kEmbeddedFileTestData --output=$@",
    exec_tools = ["//build_tools:embed_file"],
)

cc_test_mozc(
    name = "embedded_file_test",
    srcs = [
        "embedded_file_test.cc",
        ":gen_embedded_file_test_data",
    ],
    data = ["embedded_file.h"],
    deps = [
        ":embedded_file",
        ":file_stream",
        ":port",
        "//testing:gunit_main",
        "//testing:mozctest",
    ],
)

cc_library_mozc(
    name = "serialized_string_array",
    srcs = ["serialized_string_array.cc"],
    hdrs = ["serialized_string_array.h"],
    deps = [
        ":file_stream",
        ":logging",
        ":port",
        ":util",
        "@com_google_absl//absl/strings",
    ],
)

cc_test_mozc(
    name = "serialized_string_array_test",
    srcs = ["serialized_string_array_test.cc"],
    deps = [
        ":port",
        ":serialized_string_array",
        "//testing:gunit_main",
        "@com_google_absl//absl/strings",
    ],
)

cc_library_mozc(
    name = "status",
    srcs = [
        "status.cc",
    ],
    hdrs = [
        "status.h",
        "statusor.h",
    ],
    deps = select_mozc(
        default = [
            "@com_google_absl//absl/status",
        ],
        oss = [
            ":logging",
            ":flags",
        ],
    ),
)

cc_test_mozc(
    name = "status_test",
    srcs = [
        "status_test.cc",
        "statusor_test.cc",
    ],
    deps = [
        ":status",
        "//testing:gunit_main",
        "@com_google_absl//absl/memory",
    ],
)
