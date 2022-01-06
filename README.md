Github-CI:
[![Build Status][github_linux_status]][github_linux_link]
[![Build Status][github_macos_status]][github_macos_link]
[![Build Status][github_windows_status]][github_windows_link]

[github_linux_status]: https://github.com/Mizux/bazel-cpp/actions/workflows/amd64_linux.yml/badge.svg
[github_linux_link]: https://github.com/Mizux/bazel-cpp/actions/workflows/amd64_linux.yml
[github_macos_status]: https://github.com/Mizux/bazel-cpp/actions/workflows/amd64_macos.yml/badge.svg
[github_macos_link]: https://github.com/Mizux/bazel-cpp/actions/workflows/amd64_macos.yml
[github_windows_status]: https://github.com/Mizux/bazel-cpp/actions/workflows/amd64_windows.yml/badge.svg
[github_windows_link]: https://github.com/Mizux/bazel-cpp/actions/workflows/amd64_windows.yml

# Introduction
<nav for="project"> |
<a href="#requirement">Requirement</a> |
<a href="#codemap">Codemap</a> |
<a href="#dependencies">Dependencies</a> |
<a href="#build">Build</a> |
<a href="#tutorial">Tutorial</a> |
<a href="ci/README.md">CI</a> |
<a href="#appendices">Appendices</a> |
<a href="#license">License</a> |
</nav>

Bazel C++ sample with tests and GitHub CI support.

This project should run on GNU/Linux, MacOS and Windows.

## Build
To build this example you should use:

* on UNIX:
  ```sh
  bazel build --cxxopt=-std=c++17 //...:all
  ```

* on Windows when using MSVC:
  ```sh
  bazel build --cxxopt="-std:c++17" //...:all
  ```

## Running Tests
To build this example you should use:

* on UNIX:
  ```sh
  bazel test --cxxopt=-std=c++17 //...:all
  ```

* on Windows when using MSVC:
  ```sh
  bazel test --cxxopt="-std:c++17" //...:all
  ```

## Tutorial
### Integrating Googletest everywhere
The official documentation on integrating Googletest just **doesn't work for Windows** and is plain wrong (-_-;)<br>
e.g. using UNIX only include path syntax in copts `-Ipath`, using UNIX only link option in linkopts `-pthread`,
using a custom `gtest.BUILD` while a working multi-platform `BUILD.bazel` is already provided by google/googletest...<br>
ref: https://docs.bazel.build/versions/main/cpp-use-cases.html#including-external-libraries

Thanksfully after few tests, I manage to make it works on all [GitHub hosted runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners)

1. First use the git version of googletest.<br>
   WORKSPACE:
   ```bazel
   load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

    git_repository(
    name = "com_google_googletest",
    commit = "703bd9c", # release-1.10.0
    remote = "https://github.com/google/googletest.git",
   )
   ```
2. Use `@com_google_googletest` (as defined in the googletest's WORKSPACE instead of the `@gtest`) and the `gtest_main` target.<br>
   test/BUILD:
   ```bazel
   cc_test(
    name = "hello-test",
    srcs = ["hello-test.cc"],
    copts = ["-Iexternal/gtest/include"],
    deps = [
        "@com_google_googletest//:gtest_main",
        "//main:hello-greet",
    ],
   )
   ```

### Visibility
In Bazel, subdirectories containing BUILD files are known as packages.<br>
The new property `visibility` will tell Bazel which package(s) can reference this target, in this case the `//main` package can use `hello-time` library. 

lib/BUILD:
```bazel
cc_library(
    name = "hello-time",
    srcs = ["hello-time.cc"],
    hdrs = ["hello-time.h"],
    visibility = ["//main:__pkg__"],
)
```

### local dependencies
To use our `hello-time` libary, an extra dependency is added in the form of `//path/to/package:target_name`, in this case, it's `//lib:hello-time`.

main/BUILD:
```bazel
cc_binary(
    name = "hello-world",
    srcs = ["hello-world.cc"],
    deps = [
        ":hello-greet",
        "//lib:hello-time",
    ],
)
```

## CI Setup
Please take a look at [.github/workflows](.github/workflows) to find the configuration file for each jobs.

To install *bazel* on each hosted runner, follow these links:
ref: https://docs.github.com/en/actions/using-github-hosted-runners/customizing-github-hosted-runners#installing-software-on-windows-runners

* Linux (Ubuntu latest LTS) -> `apt-get install bazel`<br>
  ref: https://docs.bazel.build/versions/main/install-ubuntu.html<br>
  (as of 06/2021 Ubuntu 20.04 LTS is still not supported according to the doc...)
* MacOS -> `brew install bazel`<br>
  ref: https://formulae.brew.sh/formula/bazel#default
* Windows -> `choco install bazel`<br>
  ref: https://community.chocolatey.org/packages/bazel/

## Appendices
Few links on the subject...

### Resources
Project layout:
* The Pitchfork Layout Revision 1 (cxx-pflR1)

Bazel:
* https://docs.bazel.build/versions

### Misc
Image has been generated using [plantuml](http://plantuml.com/):
```bash
plantuml -Tsvg docs/{file}.dot
```
