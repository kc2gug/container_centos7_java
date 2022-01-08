FROM kc2gug/centos_core:latest

COPY openjdk-17.0.1_linux-x64_bin.tar.gz /
RUN tar -zxf openjdk-17.0.1_linux-x64_bin.tar.gz && rm -rf /openjdk-17.0.1_linux-x64_bin.tar.gz

ENTRYPOINT ["/bin/bash"]

