# DRBD 9.0.30-1
- The purpose of this page is to understand the brilliant replication software DRBD with source code.
  - Kernel module source code: https://github.com/LINBIT/drbd
  - The target verson is [9.0.30-1](https://github.com/LINBIT/drbd/releases/tag/drbd-9.0.30-1).
  - The original source code: https://linbit.com/linbit-software-download-page-for-linstor-and-drbd-linux-driver/#drbd
- The target OS is ~~Ubuntu 20.04.2 LTS (5.4.0-80-generic)~~ openSUSE Leap 15.3 (5.3.18-59.16-default).
- The original source code and the source codes after you run make command are different.
  - The original source code.
    ```c
    static const struct block_device_operations drbd_ops = {
            .owner          = THIS_MODULE,
            .submit_bio     = drbd_submit_bio,
            .open           = drbd_open,
            .release        = drbd_release,
    };
    ```
  - The source code after you run make command on Ubuntu 20.04.2 LTS (5.4.0-80-generic)
    ```c
    static const struct block_device_operations drbd_ops = {
            .owner          = THIS_MODULE,
            .open           = drbd_open,
            .release        = drbd_release,
    };
    ```
    - drbd_submit_bio has gone...

## Drivers
### drbd.ko
#### Initialize
1. module_init
1. [drbd_init](#drbd_init)

### drbd_transport_tcp.ko
#### Initialize
1. module_init
1. dtt_initialize

## Struct
```c
static const struct block_device_operations drbd_ops = {
        .owner          = THIS_MODULE,
        .submit_bio     = drbd_submit_bio,
        .open           = drbd_open,
        .release        = drbd_release,
};
```
- drbd_ops
  - [drbd_submit_bio](#drbd_submit_bio)
  - [drbd_open](#drbd_open)

## Function

### drbd_init

### drbd_submit_bio
1. [__drbd_make_request](#__drbd_make_request)

### __drbd_make_request
1. drbd_request_prepare
1. [drbd_send_and_submit](#drbd_send_and_submit)

### drbd_send_and_submit
1. 

### drbd_open

