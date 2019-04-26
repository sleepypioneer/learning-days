## Building a Python app with Bazel

* WORKSPACE file in root

Here are all the archive/ git repositories for python the main ones would be the `io_bazel_rules_python` but you may also want the `io__bazel_rules_docker`

In the root folder you will also need a `BUILD.bazel` file for now this can remain empty.

In the application folder you will also need a `BUILD.bazel` file. Here you define your rules. 

```
py_binary(
    name = "server",
    srcs = ["main.py"],
    main = "main.py",
    deps = [
        # This takes the name as specified in requirements.txt
        requirement("requests"),
        requirement("prometheus_client"),
    ],
    python_version = "PY3",
  )
```

Here we are stipulating we want to use Python 3 to get this to run correctly at runtime we also need to add a runtime rule *(otherwise you can hit problems where the Python being used is not the one stipulated)*

```
py_runtime(
    name = "myruntime",
    interpreter_path = select({
        # Update paths as appropriate for your system.
        "@bazel_tools//tools/python:PY2": "/usr/bin/python",
        "@bazel_tools//tools/python:PY3": "/usr/bin/python3",
    }),
    files = [],
)
```

We can then run this with

`bazel run //python_server:server --experimental_better_python_version_mixing --python_top=//python_server:myruntime`

We can also create a `py3_image` rule from the docker rules.

```
py3_image(
    name = "server.image",
    srcs = ["main.py"],
    main = "main.py",
    deps = [
        # This takes the name as specified in requirements.txt
        requirement("requests"),
        requirement("prometheus_client"),
    ],
    python_version = "PY3",
)
```

and `py_test` rules to run our tests through Bazel

```
py_test(
    name = "server_test",
    srcs = ["main_test.py"],
    deps = [":server"],
)
```
