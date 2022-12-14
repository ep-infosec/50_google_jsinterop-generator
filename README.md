# JsInterop Generator &middot; [![Build Status](https://github.com/google/jsinterop-generator/actions/workflows/ci.yaml/badge.svg)](https://github.com/google/jsinterop-generator/actions/workflows/ci.yaml)

The jsinterop generator is a java program that takes closure extern files as input and generates
Java classes annotated with [JsInterop annotations](https://goo.gl/agme3T).

> *This project is used for building Elemental2.
Any other uses are experimental. You can use it to generate java APIs for other javascript libraries but we don't provide any official support. Feel free to open issues on the github issue tracker, though.*

Run with Bazel
---------------
If your project uses [Bazel](https://bazel.build). You can use `jsinterop_generator` rule to generate java code.

You need to add this repository as an external dependency in your `WORKSPACE` file

    new_http_archive(
      name = "com_google_jsinterop_generator",
      url="https://github.com/google/jsinterop-generator/archive/master.zip",
      strip_prefix="jsinterop-generator-master",
    )

and then define a `jsinterop_generator` target in your 'BUILD' file

    load("@com_google_jsinterop_generator//:jsinterop_generator.bzl", "jsinterop_generator")

    jsinterop_generator(
        name = "my_thirdparty_lib",
        srcs = ["my_externs.js"],
    )

You can now directly depend on `:my_thirdparty_lib` target in your `java_library` rules or build the jar files with `bazel build //path/to/your/BUILD/file/directory:my_thirdparty_lib`.
The jar files with the generated source will created in `bazel-bin/path/to/your/BUILD/file/directory`.

Run as a standalone java program
---------------------------------

### Build the generator from source

- Install [Bazelisk](https://github.com/bazelbuild/bazelisk):

```shell
    $ npm install -g @bazel/bazelisk
    $ alias bazel=bazelisk
```
- Clone this git repository:
  ```shell
  $ git clone https://github.com/google/jsinterop-generator.git
  ```
- Build the binary:
  ```shell
      $ cd jsinterop-generator
      $ bazel build //java/jsinterop/generator/closure:ClosureJsinteropGenerator_deploy.jar
  ```

The generated jar file can be found at `bazel-bin/java/jsinterop/generator/closure/ClosureJsinteropGenerator_deploy.jar`

### Or download the generator
TODO(dramaix): provides link to download the generator.

### Run the generator
Now you have the jar file, just invoke

    java -jar /path/to/ClosureJsinteropGenerator_deploy.jar [options] externs_file...

List of required `options` :

Option | Meaning
------ | -------
--output _file_ | Path to the jar file that will contain the generated java classes
--output_dependency_file _file_ | Path to the dependency file generated by the generator.
--package_prefix _string_ | Prefix used when we build the java package
--extension_type_prefix _string_ | Value used for prefixing extension types. It's a good practice to pass the name of the library in upper camel case.


Contributing
------------
Please refer to [the contributing document](CONTRIBUTING.md).

Licensing
---------
Please refer to [the license file](LICENSE).

Disclaimer
----------
This is not an official Google product.

