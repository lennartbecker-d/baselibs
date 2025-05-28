ARG BASE_IMAGE=debian:12

FROM ${BASE_IMAGE}
USER root

# Optimization of installs
ENV DEBIAN_FRONTEND=noninteractive

# Definition of arguments for GCC and LLVM Version
ARG GCC_VERSION=11
ARG LLVM_VERSION=16

#Installation of c++ and utility libraries
#For C++ -> g++ compiler (in specified version), clang-tidy & clang-format (in specified LLVM version), googletest & gcovr for c++ testing
#For Detailed design -> graphviz default-jre (we can preview puml files)
#Utilities -> git, git-lfs and wget
RUN apt-get update \
    && apt-get -y install wget git git-lfs graphviz default-jre libtinfo5 g++-${GCC_VERSION} gdb googletest gcovr cmake clang-${LLVM_VERSION} clang-tidy-${LLVM_VERSION} clang-format-${LLVM_VERSION} doxygen\
    && apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/*

#specifying default versions via update alternatives functionality
RUN update-alternatives --install /usr/bin/clang-tidy clang-tidy /usr/bin/clang-tidy-${LLVM_VERSION} 100 && update-alternatives --set clang-tidy /usr/bin/clang-tidy-${LLVM_VERSION}
RUN update-alternatives --install /usr/bin/clang-format clang-format /usr/bin/clang-format-${LLVM_VERSION} 100 && update-alternatives --set clang-format /usr/bin/clang-format-${LLVM_VERSION}
RUN update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-${GCC_VERSION} 100 && update-alternatives --set g++ /usr/bin/g++-${GCC_VERSION}

#installation of bazelisk, a bazel versioning tool
RUN wget https://github.com/bazelbuild/bazelisk/releases/download/v1.26.0/bazelisk-linux-amd64 && chmod +x bazelisk-linux-amd64 && mv bazelisk-linux-amd64 /usr/local/bin/bazel