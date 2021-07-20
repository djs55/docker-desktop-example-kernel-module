# docker-desktop-example-kernel-module
This is an example showing how to build and insert a kernel module on Docker Desktop.

The example is taken from [the linuxkit test case](https://github.com/linuxkit/linuxkit/blob/ad809fa3b6d133a04bf4f49f2b1e3b5f77616f6a/test/cases/020_kernel/111_kmod_5.4.x/src/hello_world.c).

## Preliminaries

Install [Docker Desktop stable](https://hub.docker.com/editions/community/docker-ce-desktop-windows/).
I am using 3.5.2.

If you are running Windows, switch to the Hyper-V based engine (not WSL 2). The kernel
version used in Docker Desktop 3.5.2 is 5.10.47 whereas Microsoft currently ships 4.19.104.

To check your kernel version:
```
% docker run alpine uname -a
Linux 4eb10ad079ef 5.10.25-linuxkit #1 SMP PREEMPT Tue Mar 23 09:24:45 UTC 2021 aarch64 Linux
```

## Build the module

```
% cd kernel
% docker build -t kernel-module .
```

## Load the kernel module

```
PS > docker run -it --privileged kernel-module
/kmod # insmod example.ko
/kmod # lsmod
Module                  Size  Used by    Tainted: G
example                16384  0
xfrm_user              36864  1
xfrm_algo              16384  1 xfrm_user
bpfilter               36864  0
hv_sock                16384  2
vsock                  36864  3 hv_sock
/kmod # dmesg
...
[  511.274927] Hello LinuxKit
```

## Considerations

Note this example image contains the whole build environment. In real life you would copy the module
to a small clean image, see [multi-stage builds](https://www.docker.com/blog/multi-stage-builds/).
