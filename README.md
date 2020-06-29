# luks-2fa-dracut

This module for dracut allows 2FA on LUKS during boot. It doesn't use YubiKey and it doesn't require any special media. All you need is a second LUKS volume. Obviously the latter should be on a removable device (e.g. a SD card) and it should be kept secure to nullify password recorder or shoulder surfer attacks and subsequent clone/steal of the target device/computer.
<br/>
However this module, like YubiKey, doesn't protect you from physical attack. About this, another layer is needed (for info: https://blog.invisiblethings.org/2011/09/07/anti-evil-maid.html). Anyway, in case of emergency, it's easier to destroy the SD card than the drive on your motherboard...

## How it works ?

A target LUKS volume will be opened when the user provides a password for another LUKS volume on which there is a key for the first volume. So the user provides "something that has" (e.g. an SD card) and "something that knows" (i.e. the password to unlock the SD card volume).
<br/>
The relation between the target volume and the keyfile volume is specified with a kernel cmdline parameter:
```
rd.luks.2fa=UUID=keyfile_uuid:keyfile_path:UUID=target_uuid[:timeout]
```
These parameters will be translated by the [systemd generator](https://github.com/raffaeleflorio/luks-2fa-dracut/blob/master/96luks-2fa/luks-2fa-generator.sh) into a systemd.service.

## How to install/remove ?
### Installation
Clone this repo:
```
$ git clone https://github.com/raffaeleflorio/luks-2fa-dracut.git
```
You can check commit's signatures using my key: [from raffaeleflorio.github.io](https://raffaeleflorio.github.io/resources/pgp.asc) or [from pgp.mit.edu](https://pgp.mit.edu/pks/lookup?op=get&search=0x0deff00a47cf317f).

Install, a new initramfs will be created with `dracut -fv`:

```
# make install
```

Then you need to configure your LUKS volumes add your `rd.luks.2fa` to the kernel cmdline through the bootloader file. For example `/etc/default/grub` if you use GRUB.

### Removal
A new initramfs will be created with `dracut -fv`:

```
# make clean
```

# SD driver

If you want use SD you should add the `mmc_block` drivers to the initramfs. You can achieve it with the addition of: `add_drivers+=" mmc_block "` to `/etc/dracut.conf.d/luks-2fa.conf`.
