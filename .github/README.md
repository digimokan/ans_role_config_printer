# ans_role_config_printer

Configure a printer for use.

[![Release](https://img.shields.io/github/release/digimokan/ans_role_config_printer.svg?label=release)](https://github.com/digimokan/ans_role_config_printer/releases/latest "Latest Release Notes")
[![License](https://img.shields.io/badge/license-MIT-blue.svg?label=license)](LICENSE.md "Project License")

## Table Of Contents

* [Purpose](#purpose)
* [Supported Operating Systems](#supported-operating-systems)
* [Quick Start](#quick-start)
    * [Use From Playbook](#use-from-playbook)
* [Role Options](#role-options)
* [Contributing](#contributing)

## Purpose

* Install main system print client. Currently, [CUPS](https://www.cups.org/) is
  used.
* Install drivers for a specific make and model printer, and configure it for
  use. See [Supported Makes and Models](../vars/main/).

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
       - name: "Install drivers and configure Brother HL-L2395DW printer"
         ansible.builtin.include_role:
           name: ans_role_config_printer
         vars:
           printer_id: "LaserBwPrinter"
           printer_make: "Brother"
           printer_model: "Brother HL-L2395DW"
           printer_location: "Upstairs Study"
           printer_ip_address: "172.2.11.20"
           printer_mac_address: "11-22-33-AA-BB-CC"
   ```

## Role Options

See the role `defaults` file, for overridable vars:

  * [defaults/main.yml](../defaults/main.yml)

Define these _required_ vars for the role:

  * `printer_id`: the short ID of the printer (use alphanumeric chars only)
  * `printer_make`: the printer make/manufacturer name (see [supported makes](../vars/main/make.yml))
  * `printer_model`: the printer model name (see [supported models](../vars/main/))
  * `printer_location`: a description of where the printer is physically located
  * `printer_ip_address`: the printer's IP address (specify as 'XX.XX.XX.XX')
  * `printer_mac_address`: the printer's MAC address (specify as 'XX-XX-XX-XX-XX-XX')

_NOTE:_ The value of `printer_id` is used by this role to track changes to the
printer's setup. If you want to change the value of `printer_id` passed to
this role (for a specific printer), first delete the printer by following these
steps:

   ```shell
   # cupsreject LaserBwPrinter
   # cupsdisable LaserBwPrinter
   # lpadmin -x LaserBwPrinter
   ```

## Contributing

* Feel free to report a bug or propose a feature by opening a new
  [Issue](https://github.com/digimokan/ans_role_config_printer/issues).
* Follow the project's [Contributing](CONTRIBUTING.md) guidelines.
* Respect the project's [Code Of Conduct](CODE_OF_CONDUCT.md).

