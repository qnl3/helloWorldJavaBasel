##########################################
## Maven package management rules setup ##
##########################################

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

RULES_JVM_EXTERNAL_TAG = "2.0.1"
RULES_JVM_EXTERNAL_SHA = "55e8d3951647ae3dffde22b4f7f8dee11b3f70f3f89424713debd7076197eaca"

http_archive(
  name = "rules_jvm_external",
  strip_prefix = "rules_jvm_external-%s" % RULES_JVM_EXTERNAL_TAG,
  sha256 = RULES_JVM_EXTERNAL_SHA,
  url = "https://github.com/bazelbuild/rules_jvm_external/archive/%s.zip" % RULES_JVM_EXTERNAL_TAG
)

load("@rules_jvm_external//:defs.bzl", "maven_install")

maven_install(
    artifacts = [
        "org.apache.tomcat.embed:tomcat-embed-core:9.0.19",
        "org.apache.tomcat.embed:tomcat-embed-jasper:9.0.19",
        "org.apache.tomcat:tomcat-jasper:9.0.19",
        "org.apache.tomcat:tomcat-jasper-el:9.0.19",
        "org.apache.tomcat:tomcat-jsp-api:9.0.19",
        "org.apache.tomcat:tomcat-servlet-api:9.0.19",
    ],
    repositories = [
        "https://repo.maven.apache.org/maven2"
    ]
)

#######################################
## Bazel Container Image Rules setup ##
#######################################

# Download the rules_docker repository at release v0.7.0
http_archive(
    name = "io_bazel_rules_docker",
    sha256 = "aed1c249d4ec8f703edddf35cbe9dfaca0b5f5ea6e4cd9e83e99f3b0d1136c3d",
    strip_prefix = "rules_docker-0.7.0",
    urls = ["https://github.com/bazelbuild/rules_docker/archive/v0.7.0.tar.gz"],
)

# OPTIONAL: Call this to override the default docker toolchain configuration.
# This call should be placed BEFORE the call to "container_repositories" below
# to actually override the default toolchain configuration.
# Note this is only required if you actually want to call
# docker_toolchain_configure with a custom attr; please read the toolchains
# docs in /toolchains/docker/ before blindly adding this to your WORKSPACE.

load("@io_bazel_rules_docker//toolchains/docker:toolchain.bzl",
    docker_toolchain_configure="toolchain_configure"
)

#docker_toolchain_configure(
#  name = "docker_config",
#  # OPTIONAL: Path to a directory which has a custom docker client config.json.
#  # See https://docs.docker.com/engine/reference/commandline/cli/#configuration-files
#  # for more details.
#  client_config="/path/to/docker/client/config",
#)

# This is NOT needed when going through the language lang_image
# "repositories" function(s).
load(
    "@io_bazel_rules_docker//repositories:repositories.bzl",
    container_repositories = "repositories",
)

container_repositories()

load(
    "@io_bazel_rules_docker//container:container.bzl",
    "container_pull",
)

# Get the java base container 
container_pull(
  name = "java_base",
  registry = "gcr.io",
  repository = "distroless/java",
  # 'tag' is also supported, but digest is encouraged for reproducibility.
  digest = "sha256:84a63da5da6aba0f021213872de21a4f9829e4bd2801aef051cf40b6f8952e68",
)
