# rtl8821cu
Linux Driver for Realtek 8821cu

# Jeff's Notes

Compiling:

```bash
make
```

Running w/ removal after hitting enter key:

```bash
sudo insmod ./8821cu.ko ; read yn ; sudo rmmod ./8821cu.ko
```

Watching kernel logs:

```
journalctl -f
```

## Status

At the moment this thing compiles, loads, and promptly errors in the kernel when
a the USB device is inserted. The kernel reports the error occurs in the file:

```
./os_dep/linux/os_intfs.c:1509
```

Which means the `rtw_cfg80211_ndev_res_register` function defined at `os_dep/linux/ioctl_cfg80211.c:7146` is erroring somewhere.

Some initial grepping tells me the kernel function [here](https://www.kernel.org/doc/html/latest/driver-api/80211/cfg80211.html#c.wiphy_register) may be the root cause of the error, so inspecting data being sent in should tell us where the driver is broken.
