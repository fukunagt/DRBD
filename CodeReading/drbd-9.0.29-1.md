# DRBD 9.0.29-1
- The purpose of this page is to understand the brilliant replication software DRBD with source code.
  - Kernel module source code: https://github.com/LINBIT/drbd
  - The target verson is [9.0.29-1](https://github.com/LINBIT/drbd/releases/tag/drbd-9.0.29-1).
  - Source Code: https://linbit.com/linbit-software-download-page-for-linstor-and-drbd-linux-driver/#drbd

## Drivers
### drbd.ko
#### Initialize
1. module_init
1. [drbd_init](#drbd_init)

### drbd_transport_tcp.ko

## Struct
```
static const struct block_device_operations drbd_ops = {
	.owner		= THIS_MODULE,
	.submit_bio	= drbd_submit_bio,
	.open		= drbd_open,
	.release	= drbd_release,
};
```
- [drbd_submit_bio](#drbd_submit_bio)
- [drbd_open](#drbd_open)

## Function
### drbd_init
1. initialize_kref_debugging
1. register_blkdev
1. drbd_genl_register
   - We cannot find the function...
1. drbd_create_mempools
   - Allocate memory for the driver.
1. create_singlethread_workqueue
1. Initialize [do_retry](#do_retry) with INIT_WORK.

### do_retry
1. list_for_each_entry_safe
1. __drbd_make_request

### drbd_submit_bio
1. __drbd_make_request

### __drbd_make_request
1. [inc_ap_bio](#inc_ap_bio)
1. [drbd_request_prepare](#drbd_request_prepare)
1. [drbd_send_and_submit](#drbd_send_and_submit)

### inc_ap_bio
1. wait_event
   - Wait I/O.

### drbd_request_prepare
- FIXME

### drbd_send_and_submit
1. spin_lock_irq
1. submit_private_bio is not NULL.
   1. [drbd_submit_req_private_bio](#drbd_submit_req_private_bio)
1. m.bio is not NULL.
   1. [complete_master_bio](#complete_master_bio)
1. spin_unlock_irq

### drbd_submit_req_private_bio
1. get_ldev
1. submit_bio_noacct
1. put_ldev

### complete_master_bio
1. bio_data_dir
1. [bio_endio](https://www.kernel.org/doc/htmldocs/filesystems/API-bio-endio.html)
   - End I/O on the whole bio.
1. dec_ap_bio

### drbd_create_device
```c
	device->md_io.page = alloc_page(GFP_KERNEL);
	if (!device->md_io.page)
		goto out_no_io_page;
```