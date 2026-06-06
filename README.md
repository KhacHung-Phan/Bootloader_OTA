# OTA Firmware Update System for Embedded Devices

## Overview

This project implements an OTA (Over-The-Air) firmware update system for embedded devices.

The system uses an ESP32 as a Web Server Gateway to receive firmware files from the user and a STM32 microcontroller as the target device. The firmware is uploaded through a web interface, stored temporarily on the ESP32, and transmitted to the STM32 via UART.

On the STM32 side, a custom Bootloader receives the firmware, verifies data integrity, writes the new firmware to Flash memory, and jumps to the application after a successful update.

## Main Features

- OTA firmware update for embedded systems
- ESP32-based Web Server Gateway
- STM32 custom Bootloader
- Firmware upload through web browser
- `.bin` firmware file handling
- UART-based firmware transmission
- CRC/Checksum data verification
- ACK/NACK handshaking mechanism
- Flash memory erase/write operation
- Application jump after successful update
- Error detection for corrupted firmware
- Recovery test for interrupted transmission

## System Architecture

The system consists of two main parts:

### ESP32 Gateway

The ESP32 acts as the network gateway and embedded web server.

Main responsibilities:

- Create a web interface for firmware upload
- Receive `.bin` firmware file from the user
- Store firmware temporarily in ESP32 Flash/SPIFFS
- Send firmware data blocks to STM32 through UART
- Receive ACK/NACK responses from STM32
- Control the update process and display update status

### STM32 Target Device

The STM32 acts as the target microcontroller that receives and runs the new firmware.

Main responsibilities:

- Run a custom Bootloader after reset
- Receive firmware data from ESP32 through UART
- Verify firmware data using CRC/Checksum
- Erase and write data to internal Flash memory
- Jump from Bootloader to user application after successful update
- Reject corrupted firmware data when an error is detected

## Hardware Components

- ESP32 development board
- STM32F103C8T6 development board
- LED indicator
- USB power supply
- Jumper wires / Breadboard

## Technologies Used

- C/C++
- ESP32
- STM32F103C8T6
- STM32 Bootloader
- UART
- HTTP Web Server
- SPIFFS
- Flash Memory
- CRC / Checksum
- ACK / NACK
- STM32CubeIDE
- Arduino IDE / PlatformIO

## Hardware Connection

| ESP32 | STM32F103C8T6 | Description |
|------|---------------|-------------|
| GPIO17 (TX2) | PA10 (RX1) | UART data from ESP32 to STM32 |
| GPIO16 (RX2) | PA9 (TX1) | UART response from STM32 to ESP32 |
| GPIO4 | RESET | Reset control signal |
| GND | GND | Common ground |

## Firmware Update Flow

1. The user connects the computer or mobile device to the same network as the ESP32.
2. The user opens the ESP32 Web Server interface through a browser.
3. The user selects a `.bin` firmware file.
4. ESP32 receives and stores the firmware file.
5. ESP32 resets STM32 and puts it into Bootloader mode.
6. ESP32 sends the firmware to STM32 block by block through UART.
7. STM32 checks the received data using CRC/Checksum.
8. STM32 sends ACK if the block is valid or NACK if an error is detected.
9. STM32 writes valid firmware data to Flash memory.
10. After the update is complete, STM32 jumps to the new application.

## Testing Scenarios

The system was tested with the following cases:

- Successful firmware update
- Corrupted firmware detection
- CRC/Checksum error detection
- UART interruption and retransmission
- STM32 application jump after successful update

## Project Structure

```text
OTA-Firmware-Update-System/
│
├── ESP32_Gateway/              # ESP32 Web Server and UART transmission code
├── STM32_Bootloader/           # STM32 custom Bootloader source code
├── STM32_Application/          # Example application firmware
├── docs/                       # Report, diagrams, and documentation
├── images/                     # Demo images and test results
└── README.md
