# LVM Disk Expansion

Expand the required disk within your virtualisation platform

### Run disk rescan <a href="#run-disk-rescan" id="run-disk-rescan"></a>

Warning

Replace `sda` with the physical identifier of the disk

```
echo 1>/sys/class/block/sda/device/rescan
```

Run `fdisk -l` to confirm the partition is at the required size, if not reboot the host

### Expand disk <a href="#expand-disk" id="expand-disk"></a>

Expand the disk within `cfdisk`

### Expand the LVM volume <a href="#expand-the-lvm-volume" id="expand-the-lvm-volume"></a>

Warning

Replace `sda1` with the physical identifier of the partition

```
sudo pvresize /dev/sda1
```

```
lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
```

```
resize2fs -p /dev/ubuntu-vg/ubuntu-lv
```

Confirm the partition has been successfully resized using `df -h`

Source: [https://docs.binarybraids.com/linux/lvm\_disk\_expand/](https://docs.binarybraids.com/linux/lvm_disk_expand/)
