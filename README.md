<!--
SPDX-FileCopyrightText: © 2020 Open Networking Foundation <support@opennetworking.org>
SPDX-License-Identifier: Apache-2.0
--!>
# pxeboot

Configures PXE and iPXE related boot scripts and images on an web server.

See the `ipxe-build` repo for building iPXE payload images.

The iPXE `boot.ipxe` script:

- Has a menu that reports system information and network status
- Gives options for installation (interactive and autoinstall) and diagnostic
  tools (currently: memtest)
  - Alternate options to help debug or supply nonfree firmware
- By default, continues boot normally after 10 seconds

Also populates the kernel, initrd, and other files needed to network boot.

For fully automated installation, separate Debian/Ubuntu preseed files are
created based on the serial number of the device (and possibly other criteria
like MAC address in the future).  Hosts are defined in the pxeboot_hosts list,
which has these options:

- `domain`: Domain extension for the host
- `hostname`: Hostname of the system
- `serial`: Serial number, must match the SMBIOS supplied serial for server to
  boot properly.
- `iface`: (optional) Network interface to use when setting up the system.
  This is primarily to work around this bug which can cause the wrong interface
  to be selected in the install process:
  https://bugs.launchpad.net/ubuntu/+source/netcfg/+bug/713385

Documentation of the preseed process can be found in these links:

- Contents of the preseed file:
  https://help.ubuntu.com/18.04/installation-guide/amd64/apbs04.html

- All preseed configuration options: https://preseed.debian.net/debian-preseed/

- iPXE docs on preseed files and kernel arguments:
  https://ipxe.org/appnote/debian_preseed

Some systems may need additional firmware to boot properly (for example, to
initialize network cards), which can be supplied during boot as another
cpio file: https://wiki.debian.org/DebianInstaller/NetbootFirmware

## Example Playbook

```yaml
- hosts: all
  vars:
    pxeboot_hosts:
      - {domain: 'example.com', hostname: 'server1', serial: 'abc123'}
      - {domain: 'example.com', hostname: 'server2', serial: 'abc123', iface: 'eno2'}
  roles:
    - pxeboot

```

## License and Author

© 2020 Open Networking Foundation <support@opennetworking.org>
License: Apache-2.0
