# 💡 Sonoff Slampher v2 flashing with ESPHome

Recently my Sonoff Slampher stoped working and It was impossible to pair it again through eWelink app. The product seems to be deprecated. So I wanted to not only use a fully local smart device but repurpose It as a smart switch instead of a smart light bulb socket since I've relaced all my light bulbs with Zigbee ones.

## 📖 Summary

So in this tutorial I'll show the following steps to reproduce the flashing of **Sonoff Slampher v2** and an optional step requiering a 3D printer in order to print a custom case for the board

1. Opening the Sonoff Slampher
2. Locating required pins to solder on
3. Setting up your YAML file
4. Flashing the board with `esphome`
5. Pairing the board with your home assistant
6. Optional: Printing a custom case for the board

### ⚠️ Warning

You're dealing with **live electricity**, either **110V** or **220V** depending on your country.
The device has **capacitors** on the board, so be careful not to short them and leave the board disconnected from the power source for at least **10 minutes** before working on it.

If you repurpose the board as a smart switch, just keep in mind that the board has a maximum load of 450w @ 2A for 220v or 220w @ 2A for 110v.

### 📋 Prerequisites

* ✅ Sonoff Slampher v2.x
* ✅ Soldering iron is **strongly** recommended to secure the connections
* ✅ Philips screwdriver
* ✅ Your favorite text editor
* ✅ ESPHome installed on your machine
* ✅ USB TTL Serial Converter (like [this one](https://www.amazon.fr/Rtinle-Adaptateur-Conversion-dinterface-Compatible/dp/B0B7RHPMT7?dib=eyJ2IjoiMSJ9.Fo8wOrhOfXGUMwzoksDvNk_kcNMyuEYydcwaN0N4ljZ801PvuEZuiievsFxyIcHXi6viNynU_jgu7JDM1mBYVYArS3zuKwpc-TMpNlqOQY14PEw7XVYd5YbMzKLkxINh2O1zsnHToLZmkl-8wAn8kThoR1mbVu-2t5QFGnTMhm8XHxUEMHgEYprJGtCVb4xpCbRRSqN_f4JW1FLxQW5khxnY7wOqaPWQQcflgYgQr9zkZZhMV9VaqtkIVQs4q2PHb8Xs0BOWUWFEsUVAfDuJRpocSUbhjmyo00FCckNlREI.XUqJVJtKCyMYcVX0V_vTmA9p61lHyfWpFF7_pNi-UAA\&dib_tag=se\&keywords=usb+ttl\&qid=1771459019\&sr=8-6))

***

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

| Pin | Description                    |
|-----|--------------------------------|
| GND | Ground                         |
| 3V3 | Power                          |
| RX  | Data (receiver)                |
| TX  | Data (transmitter)             |
| GPIO 0 | Reset on R3 unpopulated pad |

Here's a picture where you can find them:

![Step 2 - 1](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/assets/step_2/1.JPG?raw=true)

I suggest you soldering Dupont connectors to the board in order to have a more secure connection and easier to (re)use.

![Step 2 - 2](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/assets/step_2/2.JPG?raw=true)

Then solder a jumper wire to the `R3` that can be connected to the USB TTL Serial Converter's `GND` pin in order to be able to start the board in flashing mode.

![Step 2 - 3](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/assets/step_2/3.JPG?raw=true)

Once you've soldered the pins, you can connect the USB TTL Serial Converter like so:

| Pins from the board | Pins from the USB TTL Serial Converter |
|---------------------|----------------------------------------|
| GND                 | GND                                    |
| 3V3                 | VCC                                    |
| RX                  | TX                                     |
| TX                  | RX                                     |
| R3 (GPIO 0)         | GND                                    |

## 3. 📝 Setting up your YAML file

The next step is to setup your YAML file.

You can download the [`sonoff-slampher-example.yml`](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/sonoff-slampher-example.ymls) from this repository and edit it to your needs.

Here's what you need to change:

### 🔧 General settings

The name of the device and its friendly name for Home Assistant.

```yml
esphome:
  name: <your-device-name>
  friendly_name: <your-device-friendly-name>
```

### 🔐 Authentication settings with Home Assistant

The encryption key and the reboot timeout that will be used to authenticate the device with Home Assistant.

```yml
api:
  encryption:
    key: "MZjX7hoxB43MeKI8MsLdKTjzduEUjRU9sH4TGtFCmKX="
  reboot_timeout: 0s
```

### 📦 OTA settings

The OTA (Over The Air) settings are used to update the device firmware from the ESPHome dashboard.

```yml
ota:
  - platform: esphome
    password: "de82ddbe2bebb6010a721fd2451684d6"
```

### 🛜 WiFi settings

The WiFi settings are used to connect the device to your home network.

```yml
wifi:
  ssid: "<your-wifi-ssid>"
  password: "<your-wifi-password>"
  min_auth_mode: WPA2 # Required for next ESPHome updates. Please check the ESPHome documentation for more details.
```

### 📡 Access Point settings

The AP settings are used to create a local access point for the device if the WiFi connection is not available.

```yml
ap:
  ssid: "<your-ap-ssid>"
  password: "<your-ap-password>"
```

For the rest of the file, you can keep the default settings provided in the example file.

## 4. 💻 Flashing the board with `esphome`

Here you have two options, you can either use the ESPHome CLI or the ESPHome dashboard inside your Home Assistant instance.

Since I'm have issues with the ESPHome dashboard inside my Home Assistant instance, I'll use the ESPHome CLI to flash the board. But I'll show you how to do it with the dashboard as well.

First of all make sure your board is in flashing mode by connecting the USB TTL Serial Converter to the board and the GND pin to the R3 wire that you soldered on as well as the other pins as explained at the end of the [2. 🔌 Locating required pins to solder on](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/tree/main?tab=readme-ov-file#2--locating-required-pins-to-solder-on) section.

![Step 4 - 1](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/assets/step_4/1.JPG?raw=true)

## ⚠️ Warning

Make sure your USB TTL Serial Converter is in the correct voltage for your board. In most of the adapters, you'll find a switch or a jumper Dupont connector to switch the voltage between 3.3V and 5V. Make sure to use **3.3V** for the board otherwise **YOU WILL FRY YOUR BOARD**.

![Step 4 - 2](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/assets/step_4/2.JPG?raw=true)

### 📦 Using the ESPHome CLI

1. The first step is to install the ESPHome CLI.

```bash
# Using python
pip install esphome
```

```bash
# Using brew on macOS
brew install esphome
```

```bash
# Using apt on Ubuntu
sudo apt install esphome
```

*For other operating systems, please check the [ESPHome documentation](https://esphome.io/guides/installing_esphome/) for more details.*

2. Connect the USB TTL Serial Converter to your computer and the board to the USB TTL Serial Converter.

3. Type the following command to flash the board:

```bash
esphome run <your-yaml-file>.yml
```

![Step 4 - 3](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/assets/step_4/3.gif?raw=true)

4. Wait for `esphome` to download all dependencies and select the correct board and port (generally pick option 1).

![Step 4 - 4](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/assets/step_4/4.png?raw=true)

5. Wait for `esphome` to flash the board.

![Step 4 - 5](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/assets/step_4/5.gif?raw=true)

**If everything went well, you should see the following output: `INFO Successfully uploaded program.`**

6. Once the board is flashed, you can unplug the `R3` wire from the GND pin and disconnect/reconnect the USB TTL Serial Converter in order to setup it in the Home Assistant configuration.

### 🏠 Flashing the board with the Home Assistant ESPHome integration

1. Open your Home Assistant instance and go to the `ESPHome Builder` tab.

2. Click on `+ New device` on the bottom-right corner and select `Import from file` and select your YAML file created.

3. Click on `INSTALL` and wait for the device to be installed.

4. Select the board from the list of devices.

## 🔗 Pairing the board with your Home Assistant

If everything went well, you should be able to add the device to your Home Assistant instance. You'll need to copy the API key from the device settings and paste it in the Home Assistant configuration.

1. Open your Home Assistant instance and go to the `Devices & Services` section.

2. Your new device should automagically appear in the `Devices & Services` section. Click on `Add`

![Step 5 - 1](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/assets/step_5/1.png?raw=true)

3. You'll be prompted to enter the API key. Enter the API key you copied from the device settings and click on `Submit`.

4. Now you're good to go! You should be able to control the device from your Home Assistant instance.

![Step 5 - 2](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/assets/step_5/2.gif?raw=true)

## 6. 🖨️ Optional: Printing a custom case for the board

If you want to print a custom case for the board, you can use the [`sonoff-slampher-case.stl`](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/sonoff-slampher-case.stl) and [`sonoff-slampher-cover.stl`](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/sonoff-slampher-cover.stl) files from this repository. You can also download the [Fusion 360 file](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/raw/refs/heads/main/Sonoff%20Slampher%20v2.f3d) from this repository and open it with Fusion 360 to edit the design.

You'll be able to reuse the original screws to close the case.

Here's some parameters used to print the case:

| Parameter          | Value         |
|--------------------|---------------|
| Filament type      | PLA           |
| Filament color     | White         |
| Filament brand     | Amazon Basics |
| Bed temperature    | 50°C          |
| Hotend temperature | 200°C         |
| Print speed        | 150mm/s       |
| Layer height       | 0.28mm        |
| Support            | Yes           |

![Step 6 - 1](https://github.com/MisterGoodDeal/sonoff-slampher-v2-esp-home/blob/main/assets/step_6/1.JPG?raw=true)

***

## 🎉 Congratulations!

You've successfully flashed your Sonoff Slampher v2 and paired it with your Home Assistant instance.

If you need any help, feel free to open an issue on this repository or contact me via email at <mcamus.dev@gmail.com>.
