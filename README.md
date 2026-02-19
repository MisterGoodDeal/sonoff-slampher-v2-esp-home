# 💡 Sonoff Slampher v2 flashing with ESPHome

Recently my Sonoff Slampher stoped working and It was impossible to pair it again through eWelink app. The product seems to be deprecated. So I wanted to not only use a fully local smart device but repurpose It as a smart switch instead of a smart light bulb socket since I've relaced all my light bulbs with Zigbee ones.

## 📖 Summary
So in this tutorial I'll show the following steps to reproduce the flashing of **Sonoff Slampher v2** and an optional step requiering a 3D printer in order to print a custom case for the board 

1. Opening the Sonoff Slampher
2. Locating required pins to solder on
3. Setting up your YAML file
4. Flashing the board w/ `esphome`
5. Pairing the board with your home assistant
6. Optional: Printing a custom case for the board

## ⚠️ Warning 

You're dealing with **live electricity**, either **110V** or **220V** depending on your country.
The device has **capacitors** on the board, so be careful not to short them and leave the board disconnected from the power source for at least **10 minutes** before working on it.

If you repurpose the board as a smart switch, just keep in mind that the board has a maximum load of 450w @ 2A for 220v or 22Ow @ 2A for 110v.

## 📋 Prerequisites

- ✅ Sonoff Slampher v2.x
- ✅ Soldering iron is **strongly** recommended to secure the connections
- ✅ Philips screwdriver
- ✅ Your favorite text editor
- ✅ ESPHome installed on your machine
- ✅ USB TTL Serial Converter (like [this one](https://www.amazon.fr/Rtinle-Adaptateur-Conversion-dinterface-Compatible/dp/B0B7RHPMT7?dib=eyJ2IjoiMSJ9.Fo8wOrhOfXGUMwzoksDvNk_kcNMyuEYydcwaN0N4ljZ801PvuEZuiievsFxyIcHXi6viNynU_jgu7JDM1mBYVYArS3zuKwpc-TMpNlqOQY14PEw7XVYd5YbMzKLkxINh2O1zsnHToLZmkl-8wAn8kThoR1mbVu-2t5QFGnTMhm8XHxUEMHgEYprJGtCVb4xpCbRRSqN_f4JW1FLxQW5khxnY7wOqaPWQQcflgYgQr9zkZZhMV9VaqtkIVQs4q2PHb8Xs0BOWUWFEsUVAfDuJRpocSUbhjmyo00FCckNlREI.XUqJVJtKCyMYcVX0V_vTmA9p61lHyfWpFF7_pNi-UAA&dib_tag=se&keywords=usb+ttl&qid=1771459019&sr=8-6))


## 1. 🪛 Opening the Sonoff Slampher

The first step is to open the Sonoff Slampher.

1. Unplug the Sonoff Slampher from the power source and wait for at least 10 minutes to discharge the capacitors
2. Locate the two screws on the top of the case and remove them with the Philips screwdriver

![Step 1 - 1](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/assets/step_1/1.JPG?raw=true)

3. Lift the case up and make sure not to lose the screws and don't force the case off, it should come off easily. Wires are pretty strong, but don't pull them too hard. If you plan to repurpose the board as a smart switch, you can cut the wires to the desired length or desolder them from the board/sockets.

![Step 1 - 2](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/assets/step_1/2.JPG?raw=true)

## 2. 🔌 Locating required pins to solder on

The next step is to locate the required pins to solder on.

You'll need to find 5 pins on the board to solder on.

| Pin | Description |
|---|---|
|GND| Ground |
|VCC| Power |
|RX| Data (receiver) |
|TX| Data (transmitter) |
|GPIO 0| Reset on R3 unpopulated pad |

Here's a picture where you can find them:

![Step 2 - 1](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/assets/step_2/1.JPG?raw=true)

I suggest you soldering Dupont connectors to the board in order to have a more secure connection and easier to (re)use.

![Step 2 - 2](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/assets/step_2/2.JPG?raw=true)

Then solder a jumper wire to the `R3` that can be connected to the USB TTL Serial Converter's `GND` pin in order to be able to start the board in flashing mode.

![Step 2 - 3](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/assets/step_2/3.JPG?raw=true)