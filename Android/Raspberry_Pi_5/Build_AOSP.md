# 🚀 Android 15 (AOSP) for Raspberry Pi 5 - Complete Build Guide

![Android 15 Guide for Raspberry Pi](https://github.com/user-attachments/assets/6bb7617e-4240-492d-b994-ca6aa36664c4)

---
## 🖥️ System Requirements

### Minimum Hardware
- **Host Machine**: x86_64 Linux (Ubuntu 22.04 recommended)
- **CPU**: 8-core processor (16-core recommended)
- **RAM**: 32GB (64GB recommended for faster builds)
- **Storage**: 300GB+ free space (SSD/NVMe strongly recommended)
- **Network**: Stable high-speed internet connection

### Raspberry Pi 5 Requirements
- Raspberry Pi 5 board (8GB recommended)
- Class 10 UHS-I microSD card (64GB+ recommended)
- USB-C power supply (5V/3A minimum)
- HDMI display for initial setup
---
## 🛠️ Environment Setup

### 1. Install Essential Packages
```bash
sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get install -y git-core gnupg flex bison build-essential zip curl zlib1g-dev \
gcc-multilib g++-multilib libc6-dev-i386 libncurses5 lib32ncurses5-dev x11proto-core-dev \
libx11-dev lib32z1-dev libgl1-mesa-dev libxml2-utils xsltproc unzip fontconfig \
bc coreutils dosfstools e2fsprogs fdisk kpartx mtools ninja-build pkg-config python3-pip rsync
```

### 2. Install Java (OpenJDK 11)
```bash
sudo apt-get install -y openjdk-11-jdk
java -version  # Verify output shows OpenJDK 11
```

### 3. Configure Git
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global color.ui auto
```

### 4. Install and Configure Repo Tool
```bash
mkdir -p ~/bin
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
```

---
## 📥 Source Code Download

### 1. Initialize Repository
```bash
mkdir aosp-rpi5 && cd aosp-rpi5
repo init -u https://android.googlesource.com/platform/manifest -b android-15.0.0_r26 --depth=1
```

### 2. Add Raspberry Pi 5 Manifests
```bash
curl -o .repo/local_manifests/manifest_brcm_rpi.xml -L https://raw.githubusercontent.com/raspberry-vanilla/android_local_manifest/android-15.0/manifest_brcm_rpi.xml --create-dirs

curl -o .repo/local_manifests/remove_projects.xml -L https://raw.githubusercontent.com/raspberry-vanilla/android_local_manifest/android-15.0/remove_projects.xml
```

### 3. Sync Source Code
```bash
repo sync -j$(nproc) -c --no-tags --no-clone-bundle --optimized-fetch --prune
```
**Note**: This may take several hours depending on your internet connection.

---
## ⚙️ Build Configuration

### 1. Set Up Build Environment
```bash
source build/envsetup.sh
```

### 2. Select Target Configuration
```bash
lunch aosp_rpi5_car-bp1a-userdebug
```
Available targets for Raspberry Pi 5:
- `aosp_rpi5_car-bp1a-userdebug` (Automotive)
- `aosp_rpi5-bp1a-userdebug` (Standard)
- `aosp_rpi5_tv-bp1a-userdebug` (Android TV)

### 3. Verify Configuration
Expected output should show:
```
TARGET_PRODUCT=aosp_rpi5_car
TARGET_BUILD_VARIANT=userdebug
TARGET_ARCH=arm64
TARGET_ARCH_VARIANT=armv8-a
TARGET_CPU_VARIANT=cortex-a76
```
---
## 🔨 Compilation Process

### 1. Start the Build
```bash
make bootimage systemimage vendorimage -j$(nproc)
```
**Optimization Tip**: Use `-j` with 1.5x your CPU core count (e.g., 12 for 8-core CPU).

### 2. Monitor Build Progress
```bash
watch -n 60 'tail -n 20 out/error.log && echo "" && df -h'
```

### 3. Expected Output Location
Successful builds will generate images in:
```
out/target/product/rpi5/
```
---
## 💾 Image Creation

### 1. Generate Flashable Image
```bash
./rpi5-mkimg.sh
```
Successful output will show:
```
Done, created out/target/product/rpi5/RaspberryVanillaAOSP15-YYYYMMDD-rpi5.img!
```

### 2. Verify Images
```bash
ls -lh out/target/product/rpi5/*.img
```
Expected files:
- `RaspberryVanillaAOSP15-*.img` (Complete image)
- `boot.img`
- `system.img`
- `vendor.img`
---
## 📤 Flashing to SD Card

### Method 1: Using dd (Linux/macOS)
```bash
sudo dd if=out/target/product/rpi5/RaspberryVanillaAOSP15-*.img of=/dev/sdX bs=4M status=progress && sync
```
**Warning**: Replace `/dev/sdX` with your actual SD card device (check with `lsblk`).

### Method 2: Using Raspberry Pi Imager
1. Download Raspberry Pi Imager
2. Select "Custom image" option
3. Choose the generated `.img` file
4. Select your SD card
5. Click "Write"

### Method 3: Using Balena Etcher
1. Install Etcher from https://www.balena.io/etcher/
2. Select image file
3. Choose target SD card
4. Click "Flash!"
---
## 🐛 Troubleshooting

### Common Issues and Solutions

1. **Repo Sync Errors**:
   ```bash
   repo sync --fail-fast
   repo forall -c 'git reset --hard'
   ```

2. **Missing kpartx**:
   ```bash
   sudo apt-get install kpartx
   ```

3. **Java Version Conflicts**:
   ```bash
   sudo update-alternatives --config java
   sudo update-alternatives --config javac
   ```

4. **Out of Memory Errors**:
   ```bash
   export JACK_SERVER_VM_ARGUMENTS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx8g"
   ./prebuilts/sdk/tools/jack-admin kill-server
   ./prebuilts/sdk/tools/jack-admin start-server
   ```

5. **Build Fails with Ninja Errors**:
   ```bash
   export USE_NINJA=false
   make clean
   ```
---
## 🧰 Advanced Options

### 1. CCache Configuration (Faster Rebuilds)
```bash
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
ccache -M 50G  # Set cache size to 50GB
```

### 2. Debug Build
```bash
export TARGET_BUILD_TYPE=debug
```

### 3. Generate API Documentation
```bash
m doc-comment-check-docs
```

### 4. Custom Kernel Configuration
```bash
cd kernel/brcm/rpi5
make ARCH=arm64 O=out BRCM_PLATFORM=rpi5 menuconfig
```

### 5. ADB over Network
```bash
adb connect <Pi_IP>:5555
```
---
## ❓ FAQs

**Q: How long does the full build take?**
A: On an 8-core/32GB machine: 4-8 hours for first build, 1-2 hours for incremental builds.

**Q: Can I build on a Raspberry Pi 5?**
A: Not recommended - the Pi 5 lacks sufficient RAM and storage for efficient builds.

**Q: How do I update my source code?**
A: 
```bash
repo sync -j$(nproc) -c --no-tags --no-clone-bundle --optimized-fetch --prune
make clean
```

**Q: Where can I find logs for debugging?**
A: Check these locations:
- Build logs: `out/error.log`
- Kernel logs: `out/target/product/rpi5/obj/KERNEL_OBJ/`
- ADB logs: `adb logcat`

**Q: How do I enable root access?**
A: Add to `build/make/target/product/core.mk`:
```makefile
PRODUCT_PACKAGES += su
```

## 📚 Additional Resources

- [Official AOSP Documentation](https://source.android.com/docs)
- [Raspberry Pi Android Development Forum](https://www.raspberrypi.org/forums/)
- [Android Kernel Development Guide](https://source.android.com/docs/core/architecture/kernel)
