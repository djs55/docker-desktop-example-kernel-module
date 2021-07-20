# This Dockerfile extracts the kernel headers from the kernel image
# and then compiles a simple hello world kernel module against them.
# In the last stage, it creates a package, which can be used for
# testing.

FROM docker/for-desktop-kernel:5.10.25-6594e668feec68f102a58011bb42bd5dc07a7a9b AS ksrc

# Extract headers and compile module
FROM linuxkit/alpine:e2391e0b164c57db9f6c4ae110ee84f766edc430 AS build
RUN apk add build-base elfutils-dev

COPY --from=ksrc /kernel-dev.tar /
RUN tar xf kernel-dev.tar

WORKDIR /kmod
COPY ./* ./
RUN make all