# Keychron K3 Pro

![Keychron K3 Pro](https://drive.google.com/file/d/106D9XIteQUf03WZreI1-oRYJ8iSz34qm/view?usp=share_link)

A customizable 84 keys TKL keyboard.

* Keyboard Maintainer: [Keychron](https://github.com/keychron)
* Hardware Supported: Keychron K3 Pro
* Hardware Availability: [Keychron K3 Pro QMK/VIA Wireless Custom Mechanical Keyboard](https://www.keychron.com/products/keychron-k3-pro-qmk-via-wireless-custom-mechanical-keyboard)

Make example for this keyboard (after setting up your build environment):

    make keychron/k3_pro/ansi/rgb:default

Flashing example for this keyboard:

    make keychron/k3_pro/ansi/rgb:default:flash

**Reset Key**: Connect the USB cable, toggle mode switch to "Off", hold down the *Esc* key or reset button underneath space bar, then toggle then switch to "Cable".

See the [build environment setup](https://docs.qmk.fm/#/getting_started_build_tools) and the [make instructions](https://docs.qmk.fm/#/getting_started_make_guide) for more information. Brand new to QMK? Start with our [Complete Newbs Guide](https://docs.qmk.fm/#/newbs).


## Adding Unicode support

I mean adding Hungarian characters. Read QMK's [Unicode](https://docs.qmk.fm/#/feature_unicode?id=setting-the-input-mode) 
documentation. Because of lower and upper case characters it's best to use the `Unicode Map` feature. I added the á character 
as an example to [keymap.c](ansi/white/keymaps/via/keymap.c).  
I also added a 5th layer to contain the Hungarian characters and mapped the `R_ALT` key on the 4th layer to momentary trigger
this new 5th layer.

## Building the firmware

The easiest to do it with [Docker](https://docs.qmk.fm/#/getting_started_docker?id=docker-quick-start). 

## Flashing

The keyboard must be put into bootloader mode by switching it `Off` and while holding down the *Esc* key switching it to 
`Cable` mode. If the keyboard is in bootloader mode then the `lsusb` command with list a new item: 
`0483:df11 STMicroelectronics STM Device in DFU Mode`.

Flash won't work from Docker without proper `udev` rules on the host machine, or without `sudo`. 
But with the `util/docker_cmd.sh` we can step into the docker container and execute any command from there. 
So to flash from within the container we can execute this: 

```bash
dfu-util -a 0 -d 0483:DF11 -s 0x8000000:leave -D K3-Pro-ANSI-White-v1.00.bin
```
This command is written [here](https://www.keychron.com/blogs/archived/k3-pro-factory-reset-and-firmware-flash) and [here](https://docs.qmk.fm/#/flashing?id=stm32apm32-dfu).


## Problems

- during flashing sometimes an error occurs (`bytesdfu-util: Error during special command "ERASE_PAGE" get_status`).
 Just try again until it works.
- VIA won't load, `Fetching v3 definition failed`. See [here](https://www.reddit.com/r/Keychron/comments/13nmnph/received_invalid_protocol_version_from_device_and/), [here](https://www.reddit.com/r/Keychron/comments/13e707o/k3_pro_having_issues_in_via_when_compiling_my_own/) or [here](https://www.reddit.com/r/Keychron/comments/13e707o/comment/jkltm6h/).
