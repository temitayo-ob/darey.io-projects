# IMPLEMENTING WORDPRESS WEBSITE WITH LOGICAL VOLUME MANAGEMENT(LVM) STORAGE MANAGEMENT

This course is tailored specifically to gain knowledge and skills needed to deploy and maintain a robust Wordpress site on the AWS cloud platform. 

### IMPLEMENTING LVM ON LINUX SERVERS (WEB AND DATABASE SERVERS)

##### **Step 1:** Prepare a Web server 

1. Launch an EC2 instance that will serve as a "Web server". Create 3 volumes in the same AZ as your web server EC2, each of 10GIg.

2. Attach all 3 volumes one by one to your web server EC2 instance, and open the linux terminal to begin configuration.

3. `lsblk` command can be used to inspect what block devices are attached to the server, notice and take note of the device names created. Devices reside in `/dev/` directory and can be inspected with `ls /dev/`.

![lsblk](./image/lsblk.jpg)

4. `df -h` can be used to all mounts and free space on the server.

5. `gdisk` utility creates a single partition on each of the disks.

`sudo gdisk /dev/nvme1n1`

![gdisk](./image/fdisk.jpg)

follow the prompts and type in your required specifications for the partition you want to create. 

- Use `lsblk` to view the newly created partitions on the disks.

![lsblkdone](./image/lsblkdone.jpg)

6. Install the `lvm2` package using:  `sudo yum install lvm2`.

run `lvmdiskscan` command to check for available partitions.

![lvmd](./image/lvmdiskscan.jpg)

7. Use `pvcreate` utility to mark each of the 3 disks as physical volumes (PVs) to be used by LVM.

```
sudo pvcreate /dev/nvme1n1p1
sudo pvcreate /dev/nvme2n1p1
sudo pvcreate /dev/nvme3n1p1
```

8. Verify that your physical volumes have been created using `sudo pvs`

![pvs](./image/pvs.jpg)

9. Use `vgcreate` utility to add all 3 PV's to a volume group (VG). name the VG webdata-vg

`sudo vgcreate webdata-vg /dev/nvme1n1p1 /dev/nvme2n1p1 /dev/nvme3n1p1`

![vg](./image/vgcreate.jpg)

10. Verify your VG has been created using `sudo vgs`

![vgs](./image/vgs.jpg)

11. Use `lvcreate` utility to create 2 logical volumes each using half the size of the VG, named apps-lv (website data) and logs-lv (log data) respectively.

```
sudo lvcreate -n apps-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv -L 14G webdata-vg
```

12. verify the LV creation using `sudo lvs`

![lvs](./image/lvs.jpg)

13. verify the entire setup

```
sudo vgdisplay -v #view complete setup - VG, PV, and LV
sudo lsblk 
```

![complete](./image/complete.jpg)

14. Use `mkfs.ext4` to format the logical volume with the ext4 filesystem.

```
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```

![aandl](./image/apps%20and%20logs.jpg)

15. Create **/var/www/html** directory to store website files.

`sudo mkdir -p /var/www/html`

16. Create **/home/recovery/logs** directory to store backup of log data.

`sudo mkdir -p /home/recovery/logs`

17. Mount /var/www/html on apps-lv LV

`sudo mount /dev/webdata-vg/apps-lv /var/www/html/`

18. Back up existing files in the **/var/log/** directory before we mount unto the logs-lv volume, in the **/home/recovery/logs** directory created earlier using `rsync`

`sudo rsync -av /var/log/. /home/recovery/logs/`

19. Mount **/var/log/** on logs-lv LV 

`sudo mount /dev/webdata-vg/logs-lv /var/log`




