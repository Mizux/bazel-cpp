workspace(name = "org_mizux_bazelcpp")
load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

# Bazel Extensions
## Bazel Skylib rules.
git_repository(
    name = "bazel_skylib",
    tag = "1.4.1",
    remote = "https://github.com/bazelbuild/bazel-skylib.git",
)
load("@bazel_skylib//:workspace.bzl", "bazel_skylib_workspace")
bazel_skylib_workspace()

## Bazel Platforms rules.
git_repository(
    name = "platforms",
    tag = "0.0.6",
    remote = "https://github.com/bazelbuild/platforms.git",
)

git_repository(
    name = "rules_cc",
    tag = "0.0.6",
    remote = "https://github.com/bazelbuild/rules_cc.git",
)

git_repository(
    name = "com_google_googletest",
    tag = "v1.13.0",
    remote = "https://github.com/google/googletest.git",
)
