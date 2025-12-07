# 🏗️ Ultimate AOSP Developer Command Reference

---

## 🔌 Device Connection & Control

### Basic ADB Operations
| Command | Description |
|---------|-------------|
| `adb devices` | List connected devices |
| `adb kill-server && adb start-server` | Restart ADB server |
| `adb shell` | Open device shell |
| `adb root && adb shell` | Restart as root shell |
| `adb reboot` | Normal reboot |
| `adb reboot bootloader` | Reboot to bootloader |
| `adb reboot recovery` | Reboot to recovery |
| `adb tcpip 5555` | Enable TCP/IP mode |
| `adb connect <ip>` | Connect over network |

### System Control
| Command | Description |
|---------|-------------|
| `adb remount` | Remount /system as RW |
| `adb disable-verity` | Disable dm-verity |
| `adb enable-verity` | Enable dm-verity |
| `adb shell setenforce 0` | Disable SELinux |

---

## 📁 File & System Operations

### File Transfer
| Command | Description |
|---------|-------------|
| `adb push <local> <remote>` | Copy to device |
| `adb pull <remote> <local>` | Copy from device |
| `adb shell su -c "mount -o remount,rw /system"` | Remount as root |

### System Inspection
| Command | Description |
|---------|-------------|
| `adb shell getprop` | List all properties |
| `adb shell setprop <name> <value>` | Set property |
| `adb shell ls -l /` | View root files |

---

## 📦 Package & App Management

### App Operations
| Command | Description |
|---------|-------------|
| `adb install <apk>` | Install APK |
| `adb install -r <apk>` | Reinstall keeping data |
| `adb uninstall <package>` | Uninstall app |
| `adb shell pm list packages` | List packages |

### Activity Control
| Command | Description |
|---------|-------------|
| `adb shell am start -n pkg/activity` | Launch activity |
| `adb shell am force-stop <pkg>` | Force stop app |
| `adb shell am broadcast -a <action>` | Send broadcast |

### APK Inspection
| Command | Description |
|---------|-------------|
| `aapt dump badging <apk>` | Shows APK metadata |

---

## 🐞 Debugging & Logging

### Log Collection
| Command | Description |
|---------|-------------|
| `adb logcat` | View logs |
| `adb logcat -d > log.txt` | Save logs |
| `adb bugreport` | Full system report |
| `adb shell dmesg` | Kernel logs |

### Advanced Debugging
| Command | Description |
|---------|-------------|
| `adb shell perfetto --txt -c config.pbtxt` | System tracing |
| `adb shell am profile start <proc>` | Method profiling |
| `adb shell dumpsys activity` | Dumps activity stack |
---

## ⚡ Performance Analysis

### CPU/Memory
| Command | Description |
|---------|-------------|
| `adb shell top` | Process monitor |
| `adb shell dumpsys meminfo` | Memory usage |
| `adb shell cat /proc/meminfo` | System memory |

### GPU/Rendering
| Command | Description |
|---------|-------------|
| `adb shell dumpsys gfxinfo` | Frame stats |
| `adb shell setprop debug.hwui.profile true` | Enable GPU profiling |

---

## 🏗 Build System

### Build Commands
| Command | Description |
|---------|-------------|
| `source build/envsetup.sh` | Setup env |
| `lunch` | Select target |
| `make -j$(nproc)` | Full build |
| `make bootimage` | Build kernel |

### Module Building
| Command | Description |
|---------|-------------|
| `mmm <dir>` | Build module |
| `mma` | Build current dir |
| `make snod` | Quick system image |

---

## ⚡ Fastboot & Flashing

### Device Control
| Command | Description |
|---------|-------------|
| `fastboot devices` | List devices |
| `fastboot oem unlock` | Unlock bootloader |
| `fastboot flashing unlock` | New unlock syntax |

### Partition Operations
| Command | Description |
|---------|-------------|
| `fastboot flash boot boot.img` | Flash image |
| `fastboot erase system` | Erase partition |
| `fastboot boot recovery.img` | Temp boot |

---

## 🐧 Kernel Development

### Debugging
| Command | Description |
|---------|-------------|
| `adb shell dmesg -w` | Live kernel logs |
| `adb shell cat /proc/kmsg` | Kernel buffer |
| `adb shell zcat /proc/config.gz` | Kernel config |

### Performance
| Command | Description |
|---------|-------------|
| `adb shell cat /sys/class/thermal/*/temp` | Temp monitoring |
| `adb shell watch -n 1 "cat cpu*/cpufreq/scaling_cur_freq"` | CPU freq |

---

## 🔧 System Internals

### Init System
| Command | Description |
|---------|-------------|
| `adb shell cat /proc/bootconfig` | Boot config |
| `adb shell ls -l /system/etc/init/` | Init scripts |

### Binder IPC
| Command | Description |
|---------|-------------|
| `adb shell service list` | Binder services |
| `adb shell cat /sys/kernel/debug/binder/stats` | IPC stats |

---

## 💾 Recovery & Rescue

### Backup/Restore
| Command | Description |
|---------|-------------|
| `adb shell dd if=/dev/block/boot of=/sdcard/boot.img` | Backup boot |
| `fastboot flash rescue rescue.img` | Flash rescue |
| `adb pull /dev/block/bootdevice/by-name/boot boot.img` | Backup boot image |

---

## 🗂️ AOSP File Locations

| Path | Description |
|------|-------------|
| `system/core/rootdir/init.rc` | Main init |
| `frameworks/base/` | Core framework |
| `out/target/product/<device>/` | Build outputs |

---

## 🛠 Advanced Utilities

### Magisk
| Command | Description |
|---------|-------------|
| `magiskboot unpack boot.img` | Unpack boot |
| `adb shell magisk --sqlite` | Query policies |
| `repo sync -j$(nproc)` | Syncs source code |
| `adb shell input keyevent` | Simulates key presses |

### Network
| Command | Description |
|---------|-------------|
| `adb shell tcpdump -i any -w cap.pcap` | Packet capture |
| `adb shell netcfg` | Network config |