load("@bazel_tools//tools/build_defs/pkg:pkg.bzl", "pkg_tar")
load("@io_bazel_rules_docker//container:container.bzl", "container_image")
load("@io_bazel_rules_docker//contrib:test.bzl", "container_test")
#load("@io_bazel_rules_docker//java:image.bzl", "java_image")

java_binary(
    name = "Launcher",
    srcs = glob(["src/main/java/net/qnl3/launcher/Main.java"]),
    deps = [
        "@maven//:org_apache_tomcat_embed_tomcat_embed_core",
        "@maven//:org_apache_tomcat_embed_tomcat_embed_jasper",
        "@maven//:org_apache_tomcat_tomcat_jasper",
        "@maven//:org_apache_tomcat_tomcat_jasper_el",
        "@maven//:org_apache_tomcat_tomcat_jsp_api",
        "@maven//:org_apache_tomcat_tomcat_servlet_api",
    ],
    runtime_deps = [":HelloWorld"],
    main_class = "launcher.Main"
)

java_binary(
    name = "HelloWorld",
    srcs = glob(["src/main/java/net/qnl3/HelloWorld/Hello.java"]),
    main_class = "main"
)

pkg_tar(
    name = "launcher_tar",
    strip_prefix = "/bazel-bin",
    package_dir = "/",
    srcs = [":Launcher_deploy.jar"],
    mode = "0550",
    owner = '1001.1001',
)

pkg_tar(
    name = "helloworld_tar",
    strip_prefix = "/bazel-bin",
    package_dir = "/webapp/WEB-INF/classes/net/qnl3/",
    srcs = [":HelloWorld"],
    mode = "0550",
    owner = '1001.1001',
)

pkg_tar(
    name = "webapp_tar",
    strip_prefix = "/src/java/",
    package_dir = "/app",
    srcs = glob(["src/java/webapp/**/*"]),
    mode = "0550",
    owner = '1001.1001',
)

pkg_tar(
    name = "app_tar",
    strip_prefix = "/app",
    package_dir = "/app",
    deps = [":launcher_tar",":helloworld_tar",":webapp_tar"],
    mode = "0550",
    owner = '1001.1001',
)


container_image(
    name = "WebApp",
    # References container_pull from WORKSPACE (above)
    base = "@java_base//image",
    tars = [":launcher_tar",":helloworld_tar",":webapp_tar"],
    cmd = ["Launcher_deploy.jar"],
    workdir = "/app",
    user = '1001',
)

