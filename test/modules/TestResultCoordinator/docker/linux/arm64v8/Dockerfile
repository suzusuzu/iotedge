ARG base_tag=3.1-bionic-arm64v8
FROM mcr.microsoft.com/dotnet/aspnet:${base_tag}

RUN apt-get update && \
    apt-get install -y libsnappy1v5 && \
    apt-get install -y libcap2-bin && \
    rm -rf /var/lib/apt/lists/* && \
    ln -s /lib/aarch64-linux-gnu/libdl.so.2 /usr/lib/aarch64-linux-gnu/libdl.so.2 && \
    ln -s /lib/aarch64-linux-gnu/libdl.so.2 /lib/aarch64-linux-gnu/libdl.so && \
    ln -s /lib/aarch64-linux-gnu/libdl.so.2 /usr/lib/aarch64-linux-gnu/libdl.so && \
    ln -s /lib/aarch64-linux-gnu/libc.so.6 /usr/lib/aarch64-linux-gnu/libc.so.6

COPY $EXE_DIR/librocksdb/librocksdb.so.arm64 /usr/lib/librocksdb.so

# Add an unprivileged user account for running the module
RUN useradd -ms /bin/bash moduleuser
USER moduleuser

ARG EXE_DIR=.

ENV MODULE_NAME "TestResultCoordinator.dll"

WORKDIR /app

COPY $EXE_DIR/ ./

# Expose HTTP port
EXPOSE 5001/tcp

CMD echo "$(date --utc +"%Y-%m-%d %H:%M:%S %:z") Starting Module" && \
    exec /usr/bin/dotnet TestResultCoordinator.dll
