# KMonad configs
## Explanations

The files are as follow

- `*.kbd`
  - Layouts and layer definitions for keyboard

- `systemd/`
  - Service to start in order to attach KMonad always on launch

- `udev/`
  - Rules to catch exact attribute name in order to expose to `/dev/` which can be used to catch in the rule

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

## The configuration

The main changes for the layout are as follows
- Tab on press becomes tab as expected but on hold it modifies your right home rows to have arrows (i,j,k,l) becomes (up, left, down, right)
  - This behaviour exists out of the inconvenience of having to move your fingers outside the home rows when trying to do arrows based movements
  - On keyboards lower than 75% with no function keys (like the Sofle) I would also add the functionality for this layer to include TabHold + Number key to become function keys
    - Tab+1 = F1
    - Tab+2 = F2
    - ...
    - Tab+10 = F10
    - Tab+Minus = F11
    - Tab+Equals = F12

- CapsLock is Enter on press then CTRL on hold
  - I personally have no requirement for the CapsLock key and have been using it as the Enter key for a good few years now, the CTRL part only came after I discovered Split Keyboards and QMK + Via configurations
  - The CTRL key on most keyboards are actually quite far to reach and have caused me discomfort a few times therefore having the option to press it right where my pinkie rests is quite comfy
- Space is Space on tap but symbols layer on hold
  - This came out of the necessity for not having enough symbols on the Sofle board though I have grown to enjoy the multi function usage for the spacebar. By simply adding a tap hold functionality I can access my parentheses, brackets, braces and most shift number symbols with ease.
    - Space+1 = !
    - Space+5 = %
    - etc...
  - There's also the modification to homerows where I put my common programming symbols there
    - Space+J = (
    - Space+K = )
    - Space+U = [
    - Space+I = ]
    - Space+M = {
    - Space+, = }
  - Finally there are macros for common shortcuts such as Ctrl + [A,Z,X,C,V]
    - Space+A
    - Space+Z
    - Space+X
    - Space+C
    - Space+V

