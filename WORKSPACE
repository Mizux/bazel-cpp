workspace(name = "helloworld")
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "gtest",
    strip_prefix = "googletest-release-1.8.0/googletest",
    url = "https://github.com/google/googletest/archive/release-1.8.0.zip",
    #build_file = "@//:gtest.BUILD",
    #build_file = "//:gtest.BUILD",
    build_file = "//bazel:gtest.BUILD",
)

