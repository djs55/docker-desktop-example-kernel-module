# docker-desktop-example-kernel-module
This is an example showing how to build and insert a kernel module on Docker Desktop.

The example is taken from [the linuxkit test case](https://github.com/linuxkit/linuxkit/blob/ad809fa3b6d133a04bf4f49f2b1e3b5f77616f6a/test/cases/020_kernel/111_kmod_5.4.x/src/hello_world.c).

## Preliminaries

Install [Docker Desktop stable](https://hub.docker.com/editions/community/docker-ce-desktop-windows/).
I am using 2.5.0.1.

If you are running Windows, switch to the Hyper-V based engine (not WSL 2). The kernel
version used in Docker Desktop 2.5.0.1 is 5.4.39 whereas Microsoft currently ships 4.19.104.

To check your kernel version:
```
PS > docker run alpine uname -a
Linux 6ad9784a94c6 5.4.39-linuxkit #1 SMP Fri May 8 23:03:06 UTC 2020 x86_64 Linux
```

## Build the module

```
PS > cd kernel
PS > docker build -t kernel-module .
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
