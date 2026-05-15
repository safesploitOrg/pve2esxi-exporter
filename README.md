# pve2esxi-exporter

Browser-based Proxmox VE → VMware ESXi VMDK export command generator designed for migration, disaster recovery, and incident response workflows.

Generates:
- Proxmox disk validation commands
- `dd` and `qemu-img` conversion commands
- `scp` / `rsync` transfer commands
- ESXi `vmkfstools` import commands
- Complete copy-ready migration runbooks

---

## Features

- Supports:
  - LVM / LVM-thin block devices
  - qcow2 image files
- Automatic command generation
- Local browser config persistence (`localStorage`)
- Offline capable
- Single-file HTML application
- No backend required
- Copy-to-clipboard support
- ESXi thin provisioning workflow support

---

## Tested With

- Proxmox VE 9.x
- VMware ESXi 7.x

---

## Usage

1. Open `index.html`
2. Enter:
   - VMID
   - Disk number
   - Source storage path
   - ESXi host
   - Datastore path
3. Copy generated commands
4. Execute on:
   - Proxmox host
   - ESXi host

---

## Example Workflow

```bash
# Dump
dd if=/dev/pve/vm-210-disk-1 of=/vmrestore/vm-210-VMName.raw bs=4M status=progress

# Convert
qemu-img convert -f raw -O vmdk \
  -o subformat=streamOptimized,compat6 \
  /vmrestore/vm-210-VMName.raw \
  /vmrestore/vm-210-VMName.vmdk

# Transfer
scp /vmrestore/vm-210-VMName.vmdk \
  root@esxi-01:/vmfs/volumes/datastore_lon_esxi-01/proxmox/

# On ESXi
vmkfstools -i vm-210-VMName.vmdk -d thin vm-210-VMName-thin.vmdk
```

## Disclaimer

Always ensure VMs are powered off before export to avoid filesystem inconsistency or corruption.

---

## License

MIT