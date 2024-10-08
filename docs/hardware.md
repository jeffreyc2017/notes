# Hardware

## Manufacturer

- [system76](https://system76.com/)
- [Republic of Gamers](https://rog.asus.com/)
- [WD](https://www.westerndigital.com/)
- [TeamGroup](https://www.teamgroupinc.com/en/)
- [Nvidia](https://www.nvidia.com/en-us/)
- [Intel](https://www.intel.com/)
- [Corsair](https://www.corsair.com)
- [Kingston](https://www.kingston.com)

## Tools

### PC builder

- [pangoly](https://pangoly.com/en/pc-builder)
- [UserBenchmark](https://ram.userbenchmark.com/)

## Raspberry Pi

- https://www.raspberrypi.com/documentation/computers/getting-started.html#setting-up-your-raspberry-pi

### 5

#### Power efficiency

- https://www.jeffgeerling.com/blog/2023/reducing-raspberry-pi-5s-power-consumption-140x
- https://medium.com/@life-is-short-so-enjoy-it/raspberry-pi-4-reduce-halt-state-power-consumption-to-zero-7f6097fc1862
- https://raspberry.piaustralia.com.au/blogs/getting-started-with-raspberry-pi-5/improving-raspberry-pi-5-power-efficiency-with-one-simple-fix

#### Custom screen resolution

- [iPad Pro specs](https://www.apple.com/au/ipad-pro/specs/)
- https://github.com/raspberrypi/documentation/blob/develop/documentation/asciidoc/computers/configuration/display-resolution.adoc#

## Apple

### MBP

#### Battery

- [How to reset SMC](https://support.apple.com/en-gb/102605)
- [MacBook Pro shuts down at 40% battery](https://discussions.apple.com/thread/250650983?sortBy=rank)
- [coconut battery](https://www.coconut-flavour.com/coconutbattery/)
- [battery issue](https://zh.ifixit.com/Answers/View/342625/MacBook+shuts+down+randomly+with+still+battery+charge+left?srsltid=AfmBOoq9rhB8ldXyjkkDe6SEkENLp3yvxZu0jeB43MfCzIfJMhqpEA7E)

- [Reset NVRAM](https://support.apple.com/en-us/102539)

#### Portable Hard Drive

##### Seagate

[How To Mount NTFS Volumes Manually](https://kb.paragon-software.com/article/56)

```sh
#!/bin/bash

# Get the partition identifier
partition=$(diskutil list | grep "Microsoft Basic Data Seagate" | awk '{print $NF}')

echo $partition
# Check if the partition was found
if [ -z "$partition" ]; then
    echo "Partition not found"
    exit 1
fi

# Check the mounting status of the partition
if diskutil info /dev/$partition | grep "Mounted" | grep -q "Yes"; then
    echo "Partition is already mounted."

    # Prompt the user before unmounting and ejecting
    read -p "Do you want to unmount and eject the partition? (y/n) " confirm
    if [ "$confirm" = "y" ]; then
        base_disk=$(echo "$partition" | sed 's/s[0-9]*$//')
        echo "$base_disk"

        # Unmount the partition
        sudo diskutil unmountDisk /dev/$base_disk

        # Eject the partition
        sudo diskutil eject /dev/$base_disk
    else
        echo "Skipping unmount and eject."
    fi
else
    echo "Partition is not mounted. Mounting it now..."

    sudo mkdir /Volumes/ntfs

    # Mount the partition
    sudo /Library/Filesystems/ufsd_NTFS.fs/Contents/Resources/mount_ufsd_NTFS /dev/$partition /Volumes/ntfs
    echo "Partition mounted."
fi
```
