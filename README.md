# pxeboot

Configures PXE and iPXE related boot scripts and images on an web server.

Also creates preseed files used to automate OS installation.

See the `ipxe-build` repo for building iPXE payload images.

The iPXE `boot.ipxe` script:

- Has a menu that reports system information and network status
- Gives options for installation (interactive and autoinstall) and diagnostic
  tools (currently: memtest)
  - Alternate options to help debug or supply nonfree firmware
- List of images in the iPXE menu is specified by the `pxeboot_boot_images`
  variable for customizing which tools are available.
- By default, continues boot normally after 10 seconds

Also populates the kernel, initrd, and other files needed to network boot.

For fully automated installation, separate Debian/Ubuntu preseed files are
created based on the serial number of the device (and possibly other criteria
like MAC address in the future).  Hosts are defined in the ``pxeboot_hosts``
list of dicts, which each have these keys:

- `domain`: Domain extension for the host
- `hostname`: Hostname of the system
- `iface`: (optional) Network interface to use when setting up the system.
  This is primarily to work around this bug which can cause the wrong interface
  to be selected in the install process:
  https://bugs.launchpad.net/ubuntu/+source/netcfg/+bug/713385
- To allow iPXE to load a file specific to the hardware, one or both of these
  keys must be included:
  - `serial`: Device serial number, must match value given in SMBIOS
  - `mac_address`: MAC address of the network card, colon separated format

Documentation of the preseed process can be found in these links:

- Contents of the preseed file:
  https://help.ubuntu.com/18.04/installation-guide/amd64/apbs04.html

- All preseed configuration options: https://preseed.debian.net/debian-preseed/

- iPXE docs on preseed files and kernel arguments:
  https://ipxe.org/appnote/debian_preseed

Some systems may need additional firmware to boot properly (for example, to
initialize network cards), which can be supplied during boot as another
cpio file: https://wiki.debian.org/DebianInstaller/NetbootFirmware

Additional references:

https://wiki.debian.org/DebianInstaller/Preseed
https://wiki.ubuntu.com/UEFI/PXE-netboot-install
https://help.ubuntu.com/lts/installation-guide/example-preseed.txt

iPXE script examples: https://github.com/netbootxyz/netboot.xyz

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
