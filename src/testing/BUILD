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
    "cc_library_mozc",
    "select_mozc",
)

package(
    default_visibility = [
        "//:__subpackages__",
    ],
)

alias(
    name = "testing",
    actual = "gunit",
)

genrule(
    name = "gen_mozc_data_dir_header",
    srcs = [
        # Used only for getting the directory name.
        ":testing.gyp",
    ],
    outs = ["mozc_data_dir.h"],
    cmd = (
        "$(location //build_tools:embed_pathname) " +
        "--path_to_be_embedded " +
        "$$(dirname $$(dirname $<)) " +
        "--constant_name " +
        "kMozcDataDir " +
        "--output " +
        "$@"
    ),
    tools = ["//build_tools:embed_pathname"],
)

cc_library_mozc(
    name = "mozc_data_dir_header",
    hdrs = ["mozc_data_dir.h"],
)

cc_library_mozc(
    name = "gtest_main",
    testonly = 1,
    srcs = ["base/internal/gtest_main.cc"],
    deps = [
        ":googletest",
        "//base",
        "//base:flags",
        ":gunit",
    ] + select_mozc(
        client = [
            "//base:init_mozc",
        ],
        nacl = [
            "//testing:nacl_mock_module",
            "//base:init_mozc",
        ],
    ),
)

cc_library_mozc(
    name = "nacl_mock_module",
    testonly = 1,
    srcs = ["base/public/nacl_mock_module.cc"],
    hdrs = ["base/public/nacl_mock_module.h"],
    deps = select_mozc(
        nacl = [
            ":googletest",
            ":gunit",
            "//base",
            "//base:init_mozc",
            "//base:nacl_linkopts",
            "//net:http_client",
        ],
    ) + [
        "//base:logging",
        "//base:port",
    ],
    alwayslink = 1,
)

cc_library_mozc(
    name = "gunit_main",
    testonly = 1,
    hdrs = ["base/public/gunit.h"],
    deps = [":gunit"] + select_mozc(
        client = [":gtest_main"],
        default = ["//testing/base/public:gunit_main"],
    ),
)

cc_library_mozc(
    name = "gunit_prod",
    hdrs = ["base/public/gunit_prod.h"],
)

cc_library_mozc(
    name = "testing_util",
    testonly = 1,
    srcs = ["base/public/testing_util.cc"],
    hdrs = ["base/public/testing_util.h"],
    deps = [
        ":gunit",
        ":testing",
        "//base",
        "//base:logging",
        "//base:port",
        "//base/protobuf",
        "//base/protobuf:message",
        "//base/protobuf:text_format",
    ],
)

cc_library_mozc(
    name = "gunit",
    testonly = 1,
    hdrs = ["base/public/gunit.h"],
    visibility = ["//visibility:private"],
    deps = [
        ":googletest",
        "//testing/base/public:gunit_for_library_testonly",
    ],
)

cc_library_mozc(
    name = "googletest",
    testonly = 1,
    srcs = select_mozc(
        client = ["base/internal/googletest.cc"],
        default = [],
    ),
    hdrs = ["base/public/googletest.h"],
    visibility = [
        "//:__subpackages__",
    ],
    deps = select_mozc(
        client = [
            ":mozc_data_dir_header",
            "//base",
            "//base:logging",
        ],
        default = ["//testing/base/public:gunit_for_library_testonly"],
    ) + [
        "//base:file_util",
        "//base:flags",
        "//base:util",
    ],
)

cc_library_mozc(
    name = "mozctest",
    testonly = 1,
    srcs = ["base/public/mozctest.cc"],
    hdrs = ["base/public/mozctest.h"],
    visibility = [
        "//:__subpackages__",
    ],
    deps = [
        ":googletest",
        "//base",
        "//base:file_util",
        "//base:logging",
        "//base:port",
        "//base:system_util",
        "//base:util",
        "@com_google_absl//absl/strings",
    ],
)

cc_library_mozc(
    name = "gmock",
    testonly = 1,
    hdrs = ["base/public/gmock.h"],
    visibility = [
        "//:__subpackages__",
    ],
    deps = [
        "//testing/base/public:gunit_for_library_testonly",
    ],
)
