# bazel avro rules [![Build Status](https://travis-ci.org/meetup/rules_avro.svg?branch=master)](https://travis-ci.org/meetup/rules_avro)

> [Bazel](https://bazel.build/) rules for generating java sources and libraries from [avro](https://avro.apache.org/) schemas

Adapted from [meetup/rules_avro][1] to use [rules_jvm_external][2] for dependency resolution.

[1]: https://github.com/meetup/rules_avro
[2]: https://github.com/bazelbuild/rules_jvm_external

## Rules

* [avro_gen](#avro_gen)
* [avro_java_library](#avro_java_library)

## Getting started

To use the Avro rules, add the following to your projects `WORKSPACE` file

```python
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

# update these 2 values as needed
RULES_JVM_EXTERNAL_TAG = "2.1"
RULES_JVM_EXTERNAL_SHA = "515ee5265387b88e4547b34a57393d2bcb1101314bcc5360ec7a482792556f42"

http_archive(
    name = "rules_jvm_external",
    sha256 = RULES_JVM_EXTERNAL_SHA,
    strip_prefix = "rules_jvm_external-%s" % RULES_JVM_EXTERNAL_TAG,
    url = "https://github.com/bazelbuild/rules_jvm_external/archive/%s.zip" % RULES_JVM_EXTERNAL_TAG,
)

load("@rules_jvm_external//:defs.bzl", "maven_install")

AVRO_VERSION = "1.8.2"

maven_install(
    artifacts = [
        "org.apache.avro:avro-tools:jar:%s" % AVRO_VERSION,
        "org.apache.avro:avro:jar:%s" % AVRO_VERSION,
    ],
    repositories = [
        "https://repo1.maven.org/maven2",
    ],
)

# update these 2 values as needed
RULES_AVRO_COMMIT = "e3e2af73534cba1a2591747ead327298e16c3d6d"
RULES_AVRO_SHA256 = "8a35913e99e5fe7cf170f4fb72c5d5c46deebc3efa6a6437087701015f058830"

http_archive(
    name = "io_bazel_rules_avro",
    sha256 = RULES_AVRO_SHA256,
    strip_prefix = "rules_avro-%s" % RULES_AVRO_COMMIT,
    url = "https://github.com/kien-truong/rules_avro/archive/%s.zip" % RULES_AVRO_COMMIT,
)
```

Then in your `BUILD` file, just add the following so the rules will be available:

```python
load("@io_bazel_rules_avro//avro:avro.bzl", "avro_gen", "avro_java_library")
```

## avro_gen

```python
avro_gen(name, srcs, strings, encoding)
```

Generates `.srcjar` containing generated `.java` source files from a collection of `.avsc` schemas

<table class="table table-condensed table-bordered table-params">
  <colgroup>
    <col class="col-param" />
    <col class="param-description" />
  </colgroup>
  <thead>
    <tr>
      <th colspan="2">Attributes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>name</code></td>
      <td>
        <code>Name, required</code>
        <p>A unique name for this rule.</p>
      </td>
    </tr>
    <tr>
      <td><code>srcs</code></td>
      <td>
        <code>List of labels, required</code>
        <p>
          List of <code>.avsc</code> files used as inputs for code generation
        </p>
      </td>
    </tr>
    <tr>
      <td><code>strings</code></td>
      <td>
        <code>Boolean, optional</code>
        <p>use <code>java.lang.String</code> instead of <code>Utf8</code>.</p>
      </td>
    </tr>
    <tr>
      <td><code>encoding</code></td>
      <td>
        <code>String, optional</code>
        <p>set the encoding of output files.</p>
      </td>
    </tr>
  </tbody>
</table>

## avro_java_library

```python
avro_java_library(name, srcs, strings, encoding)
```

Same as above except that the outputs include those provided by `java_library` rules.

Meetup 2017

Kien Truong 2019
