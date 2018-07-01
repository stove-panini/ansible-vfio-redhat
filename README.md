vfio-redhat
===========

Enables the passthrough of PCI devices via IOMMU and the vfio-pci kernel module on RHEL/CentOS 7 and Fedora systems.

Requirements
------------

Tested on CentOS 7 and Fedora 28. This should also work on equivalent RHEL.

You will need a CPU which supports IOMMU (the generic name for Intel VT-d and AMD-Vi). Your motherboard must also support IOMMU.

Role Variables
--------------

You can specify a list of vendor-ID:device-ID sets using the **ids** variable.

See `defaults/main.yml` for an example.

Special Considerations
----------------------

After rebooting, you may want to check your IOMMU grouping like so:
```bash
#!/bin/bash
shopt -s nullglob
for d in /sys/kernel/iommu_groups/*/devices/*; do 
    n=${d#*/iommu_groups/*}; n=${n%%/*}
    printf 'IOMMU Group %s ' "$n"
    lspci -nns "${d##*/}"
done
```
Thanks, [ArchWiki](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF#Ensuring_that_the_groups_are_valid)!

Make sure that the devices you marked for passthrough are in a group with no un-passed devices (excluding bridges and root ports)!

License
-------

MIT

Author
------

Stephen Panicho (s.panicho@gmail.com)

With help from:

[Alex Williamson's VFIO blog](https://vfio.blogspot.com/)

[The ArchWiki](https://wiki.archlinux.org)
