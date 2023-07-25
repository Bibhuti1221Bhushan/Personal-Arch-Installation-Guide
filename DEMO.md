

### Mount The Partitions :

+ Mount The Root Partition ***/dev/sda3*** To ***/mnt***.

```bash
mount /dev/sda3 /mnt
```

+ Create A ***/boot/EFI*** Directory For Boot Partition.

```bash no-copy
mkdir -p /mnt/boot/EFI  
```

Mount The Boot Partition ***/dev/sda1/*** To ***/mnt/boot/EFI*** Partition.

```bash
mount /dev/sda1 /mnt/boot/EFI
```

Create a ***/home*** mountpoint:

```
mkdir /mnt/home  
```