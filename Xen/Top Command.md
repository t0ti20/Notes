# Xen Quick Reference Guide

A professional, concise reference containing the most commonly used **Xen (xl)** commands.
Useful for developers, testers, and system engineers working with Xen-based virtualization.

---

## 📘 1. System Information

### Check Xen version

```sh
xl info
```

### List running domains

```sh
xl list
```

### Show detailed host capabilities

```sh
xl info -n
```

---

## 🖥️ 2. Domain (VM) Lifecycle

### Start a domain from config

```sh
xl create <config-file.cfg>
```

### Start and attach to console

```sh
xl create <config-file.cfg> -c
```

### Shutdown domain gracefully

```sh
xl shutdown <domain>
```

### Force shutdown (destroy)

```sh
xl destroy <domain>
```

### Reboot domain

```sh
xl reboot <domain>
```

### Pause / Unpause

```sh
xl pause <domain>
xl unpause <domain>
```

---

## 🔧 3. Console & Debugging

### Attach to VM console

```sh
xl console <domain>
```

**Detach:** `Ctrl+]`

### Xen logs

```sh
xl dmesg
```

### Clear logs

```sh
xl dmesg -c
```

---

## 📊 4. Resource & Performance Management

### Show domain resource usage

```sh
xl list -l <domain>
```

### Set number of vCPUs

```sh
xl vcpu-set <domain> <num>
```

### Set memory

```sh
xl mem-set <domain> <MB>
```

---

## 💾 5. Storage Management

### Attach disk

```sh
xl block-attach <domain> '<backend> <device> <mode>'
```

Example:

```sh
xl block-attach myvm phy:/dev/sdb xvdb w
```

### Detach disk

```sh
xl block-detach <domain> <device>
```

---

## 🌐 6. Network Management

### Attach VIF

```sh
xl network-attach <domain> type=vif
```

### Detach VIF

```sh
xl network-detach <domain> <vif-id>
```

### List network interfaces

```sh
xl network-list <domain>
```

---

## 📦 7. Snapshot / Save & Restore

### Save VM state

```sh
xl save <domain> <file>
```

### Restore VM

```sh
xl restore <file>
```

### LVM snapshot example

```sh
lvcreate --snapshot --name <snap-name> <lv-path>
```

---

## 📝 8. Configuration Templates

### Minimal HVM Config

```cfg
name = "myvm"
memory = 2048
vcpus = 2
builder = "hvm"
boot = "cd"
disk = [ 'phy:/dev/sda,xvda,w' ]
vif  = [ 'bridge=xenbr0' ]
```

### Minimal PV Config

```cfg
name = "mypvvm"
memory = 1024
vcpus = 1
kernel = "/boot/vmlinuz"
ramdisk = "/boot/initrd.img"
disk = [ 'phy:/dev/sdb,xvda,w' ]
vif  = [ 'bridge=xenbr0' ]
```

---

## 🏠 9. Host Management

### Reboot Xen host

```sh
reboot
```

### Shutdown host

```sh
poweroff
```

---

## 🛠️ 10. Troubleshooting

### Show full creation logs

```sh
xl -vvv create <config>
```

### Check Xen drivers

```sh
dmesg | grep xen
```

### Check device availability

```sh
xl devd
```