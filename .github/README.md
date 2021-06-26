# ans_role_config_printer

Configure a printer for use.

[![Release](https://img.shields.io/github/release/digimokan/ans_role_config_printer.svg?label=release)](https://github.com/digimokan/ans_role_config_printer/releases/latest "Latest Release Notes")
[![License](https://img.shields.io/badge/license-MIT-blue.svg?label=license)](LICENSE.md "Project License")

## Table Of Contents

* [Purpose](#purpose)
* [Requirements](#requirements)
* [Supported Operating Systems](#supported-operating-systems)
* [Quick Start](#quick-start)
    * [Use From Playbook](#use-from-playbook)
* [Role Options](#role-options)
* [Contributing](#contributing)

## Purpose

* Install main system print client. Currently, [CUPS](https://www.cups.org/) is
  used.
* Configure a printer for use.

## Requirements

* The printer must support the
  [IPP Everywhere](https://www.pwg.org/ipp/everywhere.html) "driverless"
  printing protocol.

* Since IPP Everywhere is used, the printer must be on, and connected to the
  network, when this role is run, in order to query the printer for settings.

* See the following resources to check IPP Everywhere support for a printer:

    * [Legacy LPD vs IPP Everywhere Discussion](https://askubuntu.com/a/1102132)
    * [IPP Everywhere Official Supported Printer List](https://www.pwg.org/printers/)
      (may not be fully up-to-date)
    * [Apple AirPrint Official Supported Printer List](https://support.apple.com/en-us/HT201311#printers)
      (extensive list, and it's usable, since AirPrint depends on IPP Everywhere)

## Supported Operating Systems

* Arch Linux.

## Quick Start

### Use From Playbook

1. Create `requirements.yml` in ansible project root, and add this content:

   ```yaml
   # requirements.yml
   - src: https://github.com/digimokan/ans_role_config_printer
   ```

2. From the project root directory, install/download the role:

   ```shell
   $ ansible-galaxy install --role-file requirements.yml --roles-path ./roles --force-with-deps
   ```

   * _NOTE:_ `--force-with-deps` _ensures subsequent calls download updates_

3. Include the role like any local role, from the project playbook:

   ```yaml
   # playbook.yml
   - hosts: localhost
     connection: local
     tasks:
       - name: "Configure Brother HL-L2395DW printer"
         ansible.builtin.include_role:
           name: ans_role_config_printer
         vars:
           printer_id: "BrotherLaserBwPrinter"
           printer_location: "Upstairs Study"
           printer_ip_address: "172.2.11.20"
   ```

## Role Options

See the role `defaults` file, for overridable vars:

  * [defaults/main.yml](../defaults/main.yml)

Define these _required_ vars for the role:

  * `printer_id`: the short ID of the printer (use alphanumeric chars only)
  * `printer_location`: a description of where the printer is physically located
  * `printer_ip_address`: the printer's IP address (specify as 'XXX.XXX.XXX.XXX')

_NOTE:_ The value of `printer_id` is used by this role to track changes to the
printer's configuration. To change the value of `printer_id` passed to this role
(for a previously-configured printer), first delete the printer. You can have
this role delete the printer only (without re-configuring it), by setting the
[`remove_printer_only`](../defaults/main.yml) var. Alternately, you can delete
the printer manually:

   ```shell
   # cupsreject BrotherLaserBwPrinter
   # cupsdisable BrotherLaserBwPrinter
   # lpadmin -x BrotherLaserBwPrinter
   ```

## Contributing

* Feel free to report a bug or propose a feature by opening a new
  [Issue](https://github.com/digimokan/ans_role_config_printer/issues).
* Follow the project's [Contributing](CONTRIBUTING.md) guidelines.
* Respect the project's [Code Of Conduct](CODE_OF_CONDUCT.md).

