# Flashing STM32 Microcontrollers

What follows is the "Eric Method" for quickly and economically flashing STM32s in-circuit.
The target audience is someone new to embedded and potentially using a riff on one of our [open source firmwares](https://github.com/heuristic-industries/two-switch) in their own designs.

## Shopping list

There's a few pieces of hardware you'll need to get going; feel free to deviate if you're feeling spicy, these are just my recommendations!

- [2.54mm pitch 10 pin IDC cables](https://www.amazon.com/dp/B07FZWWGY3) (you need two cables)
- [ST-Link programmer](https://www.amazon.com/dp/B07SQV6VLZ)
- [Tag-Connect cable](https://www.tag-connect.com/product/tc2030-idc-nl)

Note that this is the 6-pin version of the Tag-Connect cable; it's convenient if you ever want to use the same cable for AVR programming as well.
The adapter board we'll talk about later maps the pinout so you can use this cable for SWD programming!

## Software Setup

### Updating the programmer

The knockoff ST-Link programmer should work out of the box, but it's a good idea to upgrade the firmware first with the official STMicro stuff.
If you find yourself doing development down the road sometimes this software can come in handy for salvaging "bricked" dev setups too.

- Grab the [ST-Link Utility](https://www.st.com/en/development-tools/stsw-link004.html)
- Plug your programmer into your computer
- Select "Firmware update" from the "ST-LINK" drop-down menu
- Follow the instructions

### Install OpenOCD

[OpenOCD](https://openocd.org/) is what we'll use to get our computer talking to the programmer.

TODO: directions

## Hardware setup

TODO
