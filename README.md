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

Note: this is Windows only 😔

- Grab the [ST-Link Utility](https://www.st.com/en/development-tools/stsw-link004.html)
- Plug your programmer into your computer
- Select "Firmware update" from the "ST-LINK" drop-down menu
- Follow the instructions

### MacOS and Linux

#### Install OpenOCD

[OpenOCD](https://openocd.org/) is what we'll use to get our computer talking to the programmer.

### Windows

We'll use the St-Link Utility installed earlier to do the flashing, so there's no additional setup required.

## Hardware setup

### Adapter PCB

If you're using the no-name Amazon special programmer I linked above, you'll need an adapter board.
For reasons I will never understand, these use a non-standard SWD pinout, and can't be connected directly with a 10-pin IDC cable.

The [adapter_pcb](adapter_pcb) directory contains a full KiCad project for such an adapter board, as well as [pre-compiled gerber and assembly files](adapter_pcb/jlcpcb/production_files) that can be submitted directly to the fab of your choice (or JLCPCB if you would like it to show up assembled).

Submitting the board for fabrication and assembly is beyond the scope of this document, but something we can help with if need be!

### Your PCB

When creating PCBs to use with this setup, the KiCad library has pre-defined symbols that will make life a _lot_ easier and remove any guess-work for these pinouts.

- Production runs: `Conn_ARM_SWD_TagConnect_TC2030-NL`
- Dev boards: `Connector:Conn_ARM_JTAG_SWD_10` (use the `Connector_IDC:IDC-Header_2x05_P2.54mm_Vertical` footprint with it)

### Connecting it all

With the programmer, adapter board, and the thing you want to actually flash in hand, plug it in like so:

- Programmer plugged into computer
- "SWD (weird)" IDC cable plugged into programmer
- 6-pin connector plugged into Tag-Connect cable (for production)
  **OR**
- "SWD (normal)" plugged into the target device (for development)

## Flashing

### MacOS and Linux

#### Install OpenOCD

##### MacOS

I like [Homebrew](https://brew.sh) as a package manager.
If you have other preferences, use those, as they probably have a package for OpenOCD already.

- Install [Homebrew](https://brew.sh) (follow directions on site)
- Open a terminal and run `brew install openocd`

##### Linux

Whatever package manager you enjoy probably has a package for OpenOCD and you probably know better than I do already.
Just install it via whatever makes you happiest, or build it from source if you're feeling spicy.

#### Actually flashing it

With OpenOCD installed, we'll use the following command to do the flashing:

```sh
openocd -f interface/stlink.cfg -f target/stm32f0x.cfg -c "program PATH_TO_BINARY verify reset exit"
```

Replace `PATH_TO_BINARY` with the file path to the binary you're trying to flash.

I'm using the `stm32f0x` target here, but you may have to change the value for other processors.

### Windows

- Open the ST-Link Utility and connect your programmer and board to the computer
- Go to Target and choose "Connect"
- Select "Program & Vefify" from the Target menu
- Select the binary file to flash

## Tips and tricks

### Flashing many units

In a production run you're probably programming large batches of devices, and continually re-running the same command for each will start to get tedious.

We can use an infinite loop in your shell to continually retry programming, which allows you to press the Tag-Connect probe into each pedal and work your way down the line without having to touch the keyboard.

Press `ctrl+c` to exit the loop and interrupt the program.

```sh
for ((;;)) {
  # your programming command here
  sleep 1
}
```
