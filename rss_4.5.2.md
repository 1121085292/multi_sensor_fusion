# third-party/ad_rss_lib

## workspace.bzl

```bash
    native.new_local_repository(
        name = "ad_rss_lib",
        build_file = clean_dep("//third_party/ad_rss_lib:ad_rss_lib.BUILD"),
        path = "/home/tang/Downloads/ad-rss-lib-4.5.2/ad_rss",
    )
```

## ad_rss_lib.BUILD

```bash
cc_binary(
    name = "libad_rss.so",
    srcs = glob([
        "generated/src/ad/rss/**/*.cpp",
        "src/**/*.cpp",
        # "tests/**/*.cpp",
        # "tests/generated/ad/rss/**/*.cpp",
    ]) + glob([
        "generated/include/ad/rss/**/*.hpp",
        "include/ad/rss/**/*.hpp",
        # "tests/test_support/*.hpp",
        "src/**/*.hpp",
    ]),
    copts = [
        "-fPIC",
        "-DSAFE_DATATYPES_EXPLICIT_CONVERSION=1",
        "-std=c++14",
        "-Wfloat-equal",
        "-Wshadow",
        "-Wswitch-default",
        "-Wenum-compare",
        "-Wformat",
        "-Wformat-security",
        "-Wall",
        "-Wextra",
        "-pedantic",
        "-Wconversion",
        "-Wsign-conversion",
    ],
    includes = [
        "generated/include",
        "include",
        "src",
        "tests/test_support",
    ],
    linkshared = True,
    deps = [
        "@ad_physics",
        "@boost",
        "@com_google_googletest//:gtest_main",
        "@spdlog",
    ],
)

cc_library(
    name = "ad_rss",
    srcs = ["libad_rss.so"],
    hdrs = glob([
        "generated/include/ad/rss/**/*.hpp",
        "include/ad/rss/**/*.hpp",
        # "tests/test_support/*.hpp",
        "src/**/*.hpp",
    ]),
    copts = [
        "-fPIC",
        "-DSAFE_DATATYPES_EXPLICIT_CONVERSION=1",
        "-std=c++14",
        "-Wfloat-equal",
        "-Wshadow",
        "-Wswitch-default",
        "-Wenum-compare",
        "-Wformat",
        "-Wformat-security",
        "-Wall",
        "-Wextra",
        "-pedantic",
        "-Wconversion",
        "-Wsign-conversion",
    ],
    includes = [
        "generated/include",
        "include",
        "src",
        # "tests/test_support",
    ],
)
```

## ad_physics

### install spdlog

- downloda
`https://github.com/gabime/spdlog/tree/936697e5b1944bf1630cfbb9e82144f8bdc32815`

- make

```bash
  # 创建构建目录并设置 CMake 配置
  mkdir build && cd build
  cmake -DCMAKE_POSITION_INDEPENDENT_CODE=ON -DCMAKE_BUILD_TYPE=Release ..

  # 编译并安装
  make
  sudo make install
  ```

- config

workspace.bzl

```bash
    native.new_local_repository(
        name = "spdlog",
        build_file = clean_dep("//third_party/spdlog:spdlog.BUILD"),
        path = "/usr/local/include",
    )
```

spdlog.BUILD

```bash
cc_library(
    name = "spdlog",
    includes = ["."],
    linkopts = [
        "-L/usr/local/lib",
        "-lspdlog",
    ],
    copts = ["-fPIC"],  # 强制使用 -fPIC
)
```

## install ad_map

- download `https://github.com/carla-simulator/map/tree/v2.6.2`

- workspace.bzl

```bash
    native.new_local_repository(
        name = "ad_physics",
        build_file = clean_dep("//third_party/ad_physics:ad_physics.BUILD"),
        path = "/usr/local/include",
    )
```

- ad_physics.BUILD

```bash
cc_library(
    name = "ad_physics",
    includes = ["."],
    linkopts = [
        "-L/usr/local/lib",
        "-lad_physics",
    ],
    deps = [
        "@spdlog//:spdlog"
    ]
)
```
