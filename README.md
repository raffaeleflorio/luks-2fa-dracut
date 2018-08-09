# luks-2fa-dracut

This module for dracut allows 2FA on LUKS during boot. It uses systemd (enabled by default in dracut) and a second LUKS volume. Obviously the latter should be on a removable device, like a SD card.

## How it works ?

A target LUKS volume will be opened when the user provides a password for another LUKS volume on which there is a key for the first volume. So the user provides "something that posssses" (e.g. an SD card) and "something that knows" (i.e. the password to unlock the SD card volume). In this way, excluding physical attacks, an attacker needs a copy of the second volume and its password to unlock the target volume. So you're protected from a shoulder surfer and from a password recorder. Unlike a vanilla LUKS setup, where an attacker needs only the password to open the target volume.
<br>
The relation between the target volume and the keyfile volume is specified with a kernel cmdline paramater:
```
rd.luks.2fa=UUID=keyfile_uuid:keyfile_path:UUID=target_uuid[:timeout]
```
These parameters will be translated by the [systemd generator](https://github.com/raffaeleflorio/luks-2fa-dracut/blob/master/96luks-2fa/luks-2fa-generator.sh) into a systemd.service.

## How to install ?

WIP