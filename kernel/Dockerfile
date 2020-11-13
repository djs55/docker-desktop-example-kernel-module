# This Dockerfile extracts the kernel headers from the kernel image
# and then compiles a simple hello world kernel module against them.
# In the last stage, it creates a package, which can be used for
# testing.

FROM linuxkit/kernel:5.4.39-f39f83d0d475b274938c86eaa796022bfc7063d2-amd64 AS ksrc

# Extract headers and compile module
FROM linuxkit/alpine:e2391e0b164c57db9f6c4ae110ee84f766edc430 AS build
RUN apk add build-base elfutils-dev

COPY --from=ksrc /kernel-dev.tar /
RUN tar xf kernel-dev.tar

WORKDIR /kmod
COPY ./* ./
RUN make all