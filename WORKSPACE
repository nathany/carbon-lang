# Part of the Carbon Language project, under the Apache License v2.0 with LLVM
# Exceptions. See /LICENSE for license information.
# SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception

workspace(name = "carbon")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

rules_python_version = "0.3.0"

# Add Bazel's python rules and set up pip.
http_archive(
    name = "rules_python",
    sha256 = "934c9ceb552e84577b0faf1e5a2f0450314985b4d8712b2b70717dc679fdc01b",
    url = "https://github.com/bazelbuild/rules_python/releases/download/%s/rules_python-%s.tar.gz" % (
        rules_python_version,
        rules_python_version,
    ),
)

load("@rules_python//python:pip.bzl", "pip_install")

# Create a central repo that knows about the pip dependencies.
pip_install(
    name = "py_deps",
    requirements = "//github_tools:requirements.txt",
)

# Configure the bootstrapped Clang and LLVM toolchain for Bazel.
load(
    "//bazel/cc_toolchains:clang_configuration.bzl",
    "configure_clang_toolchain",
)

configure_clang_toolchain(name = "bazel_cc_toolchain")

local_repository(
    name = "llvm_bazel",
    path = "third_party/llvm-bazel/llvm-bazel",
)

load("@llvm_bazel//:configure.bzl", "llvm_configure")

llvm_configure(
    name = "llvm-project",
    src_path = "third_party/llvm-project",
    src_workspace = "@carbon//:WORKSPACE",
    targets = [
        "AArch64",
        "X86",
    ],
)

load("@llvm_bazel//:terminfo.bzl", "llvm_terminfo_system")

# We require successful detection and use of a system terminfo library.
llvm_terminfo_system(name = "llvm_terminfo")

load("@llvm_bazel//:zlib.bzl", "llvm_zlib_system")

# We require successful detection and use of a system zlib library.
llvm_zlib_system(name = "llvm_zlib")

# TODO: Can switch to a normal release version when it includes:
# https://github.com/jmillikin/rules_m4/commit/b504241407916d1d6d72c66a766daacf9603cf8b
rules_m4_version = "b504241407916d1d6d72c66a766daacf9603cf8b"

http_archive(
    name = "rules_m4",
    sha256 = "e6003c5f45746a2ad01335a8526044591f2b6c5c68852cee1bcd28adc2cf452b",
    strip_prefix = "rules_m4-%s" % rules_m4_version,
    urls = ["https://github.com/jmillikin/rules_m4/archive/%s.zip" %
            rules_m4_version],
)

load("@rules_m4//m4:m4.bzl", "m4_register_toolchains")

# When building M4, disable all compiler warnings as we can't realistically fix
# them anyways.
m4_register_toolchains(extra_copts = ["-w"])

# TODO: Can switch to a normal release version when it includes:
# https://github.com/jmillikin/rules_flex/commit/1f1d9c306c2b4b8be2cb899a3364b84302124e77
rules_flex_version = "1f1d9c306c2b4b8be2cb899a3364b84302124e77"

http_archive(
    name = "rules_flex",
    sha256 = "ad1c3a1a9bdd6254df857f84f3ab4c052df6e21ce4af5d32710f2feff2abf4dd",
    strip_prefix = "rules_flex-%s" % rules_flex_version,
    urls = ["https://github.com/jmillikin/rules_flex/archive/%s.zip" %
            rules_flex_version],
)

load("@rules_flex//flex:flex.bzl", "flex_register_toolchains")

# When building Flex, disable all compiler warnings as we can't realistically
# fix them anyways.
flex_register_toolchains(extra_copts = ["-w"])

# TODO: Can switch to a normal release version when it includes:
# https://github.com/jmillikin/rules_bison/commit/478079b28605a38000eaf83719568d756b3383a0
rules_bison_version = "478079b28605a38000eaf83719568d756b3383a0"

http_archive(
    name = "rules_bison",
    sha256 = "d662d200f4e2a868f6873d666402fa4d413f07ba1a433591c5f60ac601157fb9",
    strip_prefix = "rules_bison-%s" % rules_bison_version,
    urls = ["https://github.com/jmillikin/rules_bison/archive/%s.zip" %
            rules_bison_version],
)

load("@rules_bison//bison:bison.bzl", "bison_register_toolchains")

# When building Bison, disable all compiler warnings as we can't realistically
# fix them anyways.
bison_register_toolchains(extra_copts = ["-w"])

local_repository(
    name = "brotli",
    path = "third_party/examples/brotli/original",
)

new_local_repository(
    name = "woff2",
    build_file = "third_party/examples/woff2/BUILD.original",
    path = "third_party/examples/woff2/original",
    workspace_file = "third_party/examples/woff2/WORKSPACE.original",
)

local_repository(
    name = "woff2_carbon",
    path = "third_party/examples/woff2/carbon",
)
