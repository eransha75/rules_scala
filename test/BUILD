package(default_testonly = 1)

load(
    "//scala:scala.bzl",
    "scala_binary",
    "scala_library",
    "scala_test",
    "scala_macro_library",
    "scala_repl",
    "scala_test_suite",
    "scala_library_suite",
    "scala_junit_test",
    "scala_specs2_junit_test",
)
load(
    "//scala_proto:scala_proto.bzl",
    "scalapb_proto_library",
)

# The examples below show how to combine Scala and Java rules.
# ScalaBinary is the Scala equivalent of JavaBinary.

java_binary(
    name = "JavaBinary",
    srcs = ["JavaBinary.java"],
    main_class = "scala.test.JavaBinary",
    runtime_deps = [
        ":OtherJavaLib",
        "@scala//:scala-library",
    ],
    deps = [
        ":Exported",
        ":HelloLib",
        ":OtherLib",
        ":Runtime",
    ],
)

java_binary(
    name = "JavaBinary2",
    srcs = ["JavaBinary.java"],
    main_class = "scala.test.JavaBinary",
    deps = [
        ":Exported",
        ":HelloLib",
        ":OtherLib",
        ":Runtime",
    ],
)

scala_binary(
    name = "ScalaBinary",
    srcs = ["ScalaBinary.scala"],
    main_class = "scala.test.ScalaBinary",
    print_compile_time = True,
    deps = [
        ":HelloLib",
        ":MacroTest",
    ],
)

scala_library(
    name = "HelloLib",
    srcs = ["HelloLib.scala"],
    plugins = ["@org_psywerx_hairyfotr__linter//jar"],
    deps = [
        "Exported",
        "MacroTest",
        "OtherJavaLib",
        "OtherLib",
    ],
)

scala_repl(
    name = "HelloLibRepl",
    deps = [":HelloLib"],
)

scala_test(
    name = "HelloLibTest",
    size = "small",  # Not a macro, can pass test-specific attributes.
    srcs = ["HelloLibTest.scala"],
    deps = [
        ":HelloLib",
    ],
)

# test that a library can depend on a test:
scala_library(
    name = "lib_to_test",
    srcs = ["LibToTest.scala"],
    deps = [":HelloLibTest"],
)

scala_test_suite(
    name = "HelloLibTestSuite",
    size = "small",  # Not a macro, can pass test-specific attributes.
    srcs = glob(["HelloLibTest*.scala"]),
    deps = [
        ":HelloLib",
    ],
)

scala_test(
    name = "TestFilterTests",
    size = "small",
    srcs = glob(["TestFilterTest*.scala"]),
)

scala_repl(
    name = "HelloLibTestRepl",
    deps = [":HelloLibTest"],
)

scala_library(
    name = "OtherLib",
    srcs = ["OtherLib.scala"],
    deps = ["ExportOnly"],
)

# Test of library without src
scala_library(
    name = "ExportOnly",
    exports = ["Exported"],
)

scala_library(
    name = "Exported",
    srcs = ["Exported.scala"],
    runtime_deps = ["Runtime"],
)

# Test of a grouped library
scala_library_suite(
    name = "SuiteSrcs",
    srcs = glob(["Data*.scala"]),
)

scala_library(
    name = "ConsumeSuite",
    srcs = ["SuiteUsage.scala"],
    deps = ["SuiteSrcs"],
)

scala_macro_library(
    name = "MacroTest",
    srcs = ["MacroTest.scala"],
)

scala_library(
    name = "Runtime",
    srcs = ["Runtime.scala"],
)

java_library(
    name = "OtherJavaLib",
    srcs = ["OtherJavaLib.java"],
)

scala_library(
    name = "ScalaLibResources",
    srcs = ["src/main/scala/scala/test/ScalaLibResources.scala"],
    resources = [
        "//test/data:some.txt",
        "//test/src/main/resources/scala/test:byes",
        "//test/src/main/resources/scala/test:hellos",
    ],
)

scala_library(
    name = "ResourcesStripScalaLib",
    srcs = ["src/main/scala/scala/test/ResourcesStripScalaLib.scala"],
    resource_strip_prefix = "test/",
    resources = [
        "//test/data:foo.txt",
        "//test/data:more.txt",
        "//test/src/main/resources/scala/test:generated-hello",
        "//test/src/main/resources/scala/test:more-byes",
        "//test/src/main/resources/scala/test:more-hellos",
    ],
)

scala_binary(
    name = "ScalaLibBinary",
    srcs = ["src/main/scala/scala/test/ScalaLibBinary.scala"],
    main_class = "scala.test.ScalaLibBinary",
    deps = ["ScalaLibResources"],
)

scala_binary(
    name = "ResourcesStripScalaBinary",
    srcs = ["src/main/scala/scala/test/ResourcesStripScalaBinary.scala"],
    main_class = "scala.test.ResourcesStripScalaBinary",
    deps = ["ResourcesStripScalaLib"],
)

# test that a library can depend on a binary:
scala_library(
    name = "lib_to_bin",
    srcs = ["LibToBin.scala"],
    deps = [":ScalaLibBinary"],
)

scala_repl(
    name = "ScalaLibBinaryRepl",
    deps = [":ScalaLibBinary"],
)

scala_repl(
    name = "ResourcesStripScalaBinaryRepl",
    deps = [":ResourcesStripScalaBinary"],
)

scala_repl(
    name = "ReplWithSources",
    srcs = ["A.scala"],
)

scala_library(
    name = "jar_export",
    exports = ["@com_twitter__scalding_date//jar"],
)

#Mix java scala
scala_library(
    name = "MixJavaScalaLib",
    srcs = glob(["src/main/scala/scala/test/mix_java_scala/*.scala"]) + glob([
        "src/main/scala/scala/test/mix_java_scala/*.java",
    ]),
    jvm_flags = [
        "-Xms1G",
        "-Xmx4G",
    ],
)

#needed to test java sources are compiled
scala_binary(
    name = "MixJavaScalaLibBinary",
    srcs = ["src/main/scala/scala/test/MixJavaScalaLibBinary.scala"],
    main_class = "scala.test.MixJavaScalaLibBinary",
    deps = ["MixJavaScalaLib"],
)

#Mix java scala (srcjar), much like above only the java is provided via srcjar
scala_library(
    name = "MixJavaScalaSrcjarLib",
    srcs = glob([
        "src/main/scala/scala/test/mix_java_scala/*.scala",
        # srcjar created with `jar -cfM Baz.srcjar Baz.java`
        "src/main/scala/scala/test/mix_java_scala/*.srcjar",
    ]),
    jvm_flags = [
        "-Xms1G",
        "-Xmx4G",
    ],
)

#needed to test java sources are compiled
scala_binary(
    name = "MixJavaScalaSrcjarLibBinary",
    srcs = ["src/main/scala/scala/test/MixJavaScalaLibBinary.scala"],
    main_class = "scala.test.MixJavaScalaLibBinary",
    deps = ["MixJavaScalaSrcjarLib"],
)

scala_binary(
    name = "JavaOnlySources",
    srcs = ["src/main/scala/scala/test/only_java/Alpha.java"],
    main_class = "scala.test.Alpha",
)

scala_library(
    name = "JUnitCompileTimeDep",
    srcs = ["src/main/scala/scala/test/junit/support/JUnitCompileTimeDep.scala"],
)

scala_library(
    name = "JUnitRuntimeDep",
    srcs = ["src/main/scala/scala/test/junit/support/JUnitRuntimeDep.scala"],
)

scala_junit_test(
    name = "JunitTestWithDeps",
    size = "small",
    srcs = ["src/main/scala/scala/test/junit/JunitTests.scala"],
    jvm_flags = ["-XX:HeapDumpPath=/some/custom/path"],
    suffixes = ["Test"],
    runtime_deps = [":JUnitRuntimeDep"],
    deps = [":JUnitCompileTimeDep"],
)

scala_junit_test(
    name = "JunitMultipleSuffixes",
    size = "small",
    srcs = ["src/main/scala/scala/test/junit/JunitMultipleSuffixes.scala"],
    print_discovered_classes = True,
    suffixes = [
        "E2E",
        "IT",
    ],
)

scala_junit_test(
    name = "JunitMultiplePrefixes",
    size = "small",
    srcs = ["src/main/scala/scala/test/junit/JunitMultiplePrefixes.scala"],
    prefixes = [
        "Test",
        "OtherCustomPrefix",
    ],
    print_discovered_classes = True,
)

scala_junit_test(
    name = "JunitPrefixesAndSuffixes",
    size = "small",
    srcs = [
        "src/main/scala/scala/test/junit/JunitMultiplePrefixes.scala",
        "src/main/scala/scala/test/junit/JunitMultipleSuffixes.scala",
    ],
    prefixes = ["Test"],
    print_discovered_classes = True,
    suffixes = ["E2E"],
)

scala_junit_test(
    name = "JunitJavaTest",
    size = "small",
    srcs = ["src/main/scala/scala/test/junit/JunitJavaTest.java"],
    suffixes = ["Test"],
)

scala_specs2_junit_test(
    name = "Specs2Tests",
    size = "small",
    srcs = ["src/main/scala/scala/test/junit/specs2/Specs2Tests.scala"],
    print_discovered_classes = True,
    suffixes = ["Test"],
    deps = [":JUnitCompileTimeDep"],
)

# Make sure scala_binary works in test environment
[sh_test(
    name = "Run" + "".join([binary[idx] if binary[idx].isalnum() else "_" for idx in range(len(binary))]),
    srcs = ["test_binary.sh"],
    args = ["$(location %s)" % binary],
    data = [binary if (":" in binary) else (":%s" % binary)],
) for binary in [
    "JavaBinary",
    "JavaBinary2",
    "JavaOnlySources",
    "MixJavaScalaLibBinary",
    "MixJavaScalaSrcjarLibBinary",
    "ScalaBinary",
    "ScalaLibBinary",
    "ResourcesStripScalaBinary",
    "ScalaBinary_dependent_on_binary_with_java",
    "//test/src/main/scala/scala/test/classpath_resources:classpath_resource",
]]

# Make sure scala_binary works in genrule environment
genrule(
    name = "ScalaBinaryInGenrule",
    outs = ["scala_binary_out.txt"],
    cmd = "$(location :ScalaBinary) > $@",
    tools = [":ScalaBinary"],
)

sh_test(
    name = "TestScalaBinaryInGenrule",
    srcs = ["test_binary.sh"],
    args = ["cat $(location :ScalaBinaryInGenrule)"],
    data = [":ScalaBinaryInGenrule"],
)

scala_specs2_junit_test(
    name = "data_location_expansion",
    size = "small",
    srcs = ["src/main/scala/scala/test/location_expansion/LocationExpansionTest.scala"],
    data = ["//src/java/com/google/devtools/build/lib:worker"],
    jvm_flags = ["-Dlocation.expanded=$(location //src/java/com/google/devtools/build/lib:worker)"],
    suffixes = ["Test"],
)

scala_junit_test(
    name = "JunitFiltersAbstractClassesAndInterfaces",
    size = "small",
    srcs = ["src/main/scala/scala/test/junit/JunitAbstractClassAndInterface.scala"],
    print_discovered_classes = True,
    suffixes = ["Test"],
)

scala_junit_test(
    name = "JunitFiltersInnerClasses",
    size = "small",
    srcs = ["src/main/scala/scala/test/junit/JunitInnerClass.scala"],
    prefixes = ["Test"],
    print_discovered_classes = True,
)

scala_junit_test(
    name = "JunitFiltersClassesWithoutTests",
    size = "small",
    srcs = ["src/main/scala/scala/test/junit/JunitNoTests.scala"],
    print_discovered_classes = True,
    suffixes = ["Test"],
)

scala_junit_test(
    name = "JunitIncludesClassesWithTestsFromParent",
    size = "small",
    srcs = ["src/main/scala/scala/test/junit/JunitIncludesTestHierarchies.scala"],
    print_discovered_classes = True,
    suffixes = ["Test"],
)

scala_junit_test(
    name = "JunitCustomRunner",
    size = "small",
    srcs = ["src/main/scala/scala/test/junit/JunitCustomRunnerTest.scala"],
    suffixes = ["Test"],
    deps = [":customJunitRunner"],
)

scala_junit_test(
    name = "JunitIncludesClassesWithRunWith",
    size = "small",
    srcs = ["src/main/scala/scala/test/junit/JunitIncludesRunWith.scala"],
    print_discovered_classes = True,
    suffixes = ["Test"],
)

scala_junit_test(
    name = "JunitFilterTest",
    size = "small",
    srcs = ["src/main/scala/scala/test/junit/FilterTest.scala"],
    print_discovered_classes = True,
    suffixes = ["Test"],
)

scala_library(
    name = "filesWithUtf8",
    srcs = [
        "src/main/scala/scala/test/utf8/JavaClassWithUtf8.java",
        "src/main/scala/scala/test/utf8/ScalaClassWithUtf8.scala",
    ],
)

scala_library(
    name = "customJunitRunner",
    srcs = ["src/main/scala/scala/test/junit/JunitCustomRunner.java"],
    deps = ["//external:io_bazel_rules_scala/dependency/junit/junit"],
)

# make sure making a fat jar strips signatures
java_import(
    name = "fakejar",
    jars = ["fake_sig.jar"],
)

scala_binary(
    name = "ScalaBinary_with_fake",
    srcs = ["ScalaBinary.scala"],
    main_class = "scala.test.ScalaBinary",
    deps = [
        ":HelloLib",
        ":MacroTest",
        ":fakejar",
    ],
)

py_binary(
    name = "jar_lister",
    srcs = ["jar_lister.py"],
)

sh_test(
    name = "no_sig",
    srcs = ["no_sigs.sh"],
    args = [
        "$(location //test:jar_lister)",
        "$(location //test:ScalaBinary_with_fake_deploy.jar)",
    ],
    data = [
        ":ScalaBinary_with_fake_deploy.jar",
        ":jar_lister",
    ],
)

scala_binary(
    name = "ScalaBinary_with_service_manifest_srcs",
    srcs = ["A.scala"],
    main_class = "scala.test.A",
    deps = [
        "//test/example_jars:example_jar1",
        "//test/example_jars:example_jar2",
    ],
)

scala_binary(
    name = "ScalaBinary_dependent_on_binary_with_java",
    srcs = ["src/main/scala/scala/test/BinaryDependentOnJava.scala"],
    main_class = "scala.test.BinaryDependentOnJava",
    deps = [":JavaOnlySources"],
)

scala_library(
    name = "lib_with_scala_proto_dep",
    srcs = ["TestServer.scala"],
    deps = ["//test/proto:test_proto"],
)

scala_binary(
    name = "test_scala_proto_server",
    main_class = "test.proto.TestServer",
    deps = [":lib_with_scala_proto_dep"],
)

scalapb_proto_library(
    name = "test_proto_scala_dep",
    visibility = ["//visibility:public"],
    deps = [
        ":HelloLib",
        "//test/proto:test2",
    ],
)

load(":check_statsfile.bzl", "check_statsfile")

check_statsfile("ScalaBinary")

check_statsfile("ScalaLibBinary")
