# KMonad configs
## Explanations

The files are as follow

`*.kbd`

Layouts and layer definitions for keyboard

`systemd/`

Service to start in order to attach KMonad always on launch

`udev/`

Rules to catch exact attribute name in order to expose to `/dev/` which can be used to catch in the rule

## Getting started

### Prerequisites

- Install KMonad using your favourite package manager
- Ensure you have systemd

### Create Keymaps

- Define your new layout for example `cherrystream.kbd`
- Create a new udev rules to expose the input method to `/dev/`
  - You may use `udevadm monitor --property` then disconnect and reconnect your device to find the `NAME` attribute which can be targeted within the `.rule` file
  - Map the name then symlink it to dev with the `SYMLINK+="cherrystream"`
  - This means that any time an input device matches the attribute will be exposed to `/dev/cherrystream`
  - Copy the rule file to `/etc/udev/rules.d/` folder `/etc/udev/rules.d/cherrystream.rules`
  - Reconnect the device and `ls /dev/` to test whether your udev rule is working - you should see `/dev/cherrystream`
- Create a new systemd service
  - You may reuse the `kmonad@cherrystream` and replace the latter part with your symlink token that we defined in the rule earlier - `kmonad@sofle`, `kmonad@nuphyair75`
  - Copy the new service to user defined service - in my case I am using ArchCraft which means it should be at `/etc/systemd/system/`
    - `/etc/systemd/system/kmonad@cherrystream`
  - Start the service `systemctl start kmonad@cherrystream`
  - Persist the service on every login/logout `systemctl enable kmonad@cherrystream`


