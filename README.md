<!--
SPDX-FileCopyrightText: © 2020 Open Networking Foundation <support@opennetworking.org>
SPDX-License-Identifier: Apache-2.0
--!>
# pxeboot

Configures PXE and iPXE related boot scripts and images on an web server.

The iPXE `boot.ipxe` script:

- Has a menu that reports system information and network status
- Gives options for installation (interactive and autoinstall) and diagnostic
  tools (currently: memtest)
- By default, continues boot normally after 10 seconds

Also populates the kernel, initrd, and other files needed to network boot.

For fully automated installation, separate Debian/Ubuntu preseed files are
created based on the serial number of the device (and possibly other criteria
like MAC address in the future).

Documentation of the preseed process:

- Contents of the preseed file: https://help.ubuntu.com/18.04/installation-guide/amd64/apbs04.html
- All preseed configuration options: https://preseed.debian.net/debian-preseed/

## Example Playbook

```yaml
- hosts: all
  roles:
    - pxeboot

```

## License and Author

© 2020 Open Networking Foundation <support@opennetworking.org>
License: Apache-2.0
