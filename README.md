Main goals:
- Research RFID encoding/KDF - https://github.com/SnapmakerResearchGroup/RFID
- Access root access to printer
- Implement custom RFID tag reading capabilities in "CFW"

# Hardware:

CPU: RK3562
RFID reader: FM175XX

# Firmware repository

0.9.0 firmware link: https://public.resource.snapmaker.com/firmware/U1/U1_0.9.0.121_20251106132913_upgrade.bin
SHA256: 8b4296bd7122485100607aa8ce73e60853447d016fa9affa99c1c3e7f67c7421

# Root

Users password hashes:
root:$5$eu8wDdiZh8$XX7iyka0CVnr.6rZ2CxA71A/Zuoq3Cz2k1t1WNtevA9:::::::
lava:$5$qeMHqpDjqkRkD$KcMqymy5Zu1CBhZoXmZX/oxJ4.TAWMF5OXdcNbHmIh6:::::::

Both passwords are `snapmaker`

# SSH

SSH server is dropbear (allows root login), but disabled in production builds

# Debug mode

Debug mode (that enables SSH) is enabled by generating debug file via `custom_misc gen-debug` command and then flashing result into `/dev/block/by-name/misc`. 

```shell
/usr/bin/custom_misc gen-debug
dd if=debug_misc.img of=/dev/block/by-name/misc bs=1 seek=25600 conv=notrunc
```

It will generate file with mostly empty content
```xxd
000063e0: 0000 0000 0000 0000 0000 0000 0000 0000  ................  
000063f0: 0000 0000 0000 0000 0000 0000 0000 0000  ................  
00006400: 127e 84cb 0001 030d 6462 6700 0000 0000  .~......dbg.....  
00006410: 0000 0000 0000 0000 0000 0000 0000 0000  ................  
00006420: 0000 0000 0000 0000 0000 0000 0000 0000  ................  
...
000064e0: 0000 0000 0000 0000 0000 0000 0000 0000  ................  
000064f0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
```
Where:
127E84CB - constant
0001 - debug mode enable?
030D - checksum
646267 - ASCII "dbg"

This requires root access, might be useful for easy persistent access to device (after reboot).

# MQTT

By default uses `cli0:snapmaker` credentials
