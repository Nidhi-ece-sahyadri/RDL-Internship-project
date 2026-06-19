
# Embedded Web Server using STM32 & Ethernet

## RDL Technologies Ltd. Internship Project

This repository contains the tasks completed during my internship at **RDL Technologies Ltd., Adyar, Mangalore**.

The project focuses on implementing Embedded Ethernet Web Servers using STM32 microcontrollers and Ethernet communication technologies.

---

# Project Overview

The internship project was divided into three tasks:

- **Task 1:** STM32F767ZI + LwIP HTTP Server with CGI LED Control
- **Task 2:** STM32F446RCTx + W5500 Embedded Web Server
- **Task 3:** HTML to C Byte Array Conversion using Perl
 project link https://drive.google.com/drive/folders/1RZzaBCIaGQTX-SmTGZjgAxKQBEn_lWZZ?usp=drive_link
---

# Hardware Used

- STM32F767ZI Nucleo-144 Board
- STM32F446RCTx Custom Board
- WIZnet W5500 Ethernet Controller
- RJ45 Ethernet Cable
- Router / Switch
- PC for Debugging and Browser Testing

---

# Software Used

- STM32CubeIDE
- STM32CubeMX
- STM32 HAL Drivers
- LwIP Middleware
- WIZnet ioLibrary
- Perl
- PuTTY / Serial Monitor
- Google Chrome / Firefox

---

# Task 1 – STM32 Ethernet HTTP Server using LwIP and CGI

## Overview

Task 1 focused on building an Embedded HTTP Server using the STM32F767ZI Nucleo-144 board and the LwIP TCP/IP stack. The goal was to host a web page on the STM32 and control onboard LEDs through a browser.

The project uses the Common Gateway Interface (CGI) provided by LwIP to process browser requests and perform actions inside the firmware.

---

## Objectives

- Configure Ethernet on STM32F767ZI.
- Enable LwIP middleware.
- Implement an HTTP server.
- Use CGI handlers for browser interaction.
- Control onboard LEDs remotely.

---

## Hardware Used

| Component | Description |
|------------|-------------|
| STM32F767ZI Nucleo-144 | Main Controller |
| Ethernet Cable | Network Communication |
| Router/Switch | LAN Access |
| PC/Mobile Browser | User Interface |

---

## Software Used

- STM32CubeIDE
- STM32CubeMX
- STM32Cube FW_F7
- LwIP Middleware
- Perl
- Web Browser

---

## CubeMX Configuration

### Ethernet
- Mode = RMII

### Middleware
Enable:

```text
HTTPD Server
LWIP_HTTPD_CGI
````

### GPIO Configuration

| Pin  | LED   |
| ---- | ----- |
| PB0  | Green |
| PB7  | Blue  |
| PB14 | Red   |

---

## Working Principle

1. Browser opens STM32 IP address.
2. LwIP serves index.html.
3. User selects LED states.
4. Browser sends:

```text
LEDControl.cgi?green=ON&blue=OFF&red=ON
```

5. CGI handler parses parameters.
6. Variables are updated.
7. Main loop updates GPIO outputs.
8. LEDs change state immediately.

---

## Source Files

### main.c

Contains:

* HTTP server initialization
* CGI handler registration
* GPIO control
* LED variables
* Main loop

### index.html

Contains:

* Web interface
* LED ON/OFF controls

### fsdata.c & fsdata_custom.c

Generated using:

```bash
perl makefsdata.pl
```

These files convert HTML pages into C arrays for LwIP.

---

## Main Loop

```c
MX_LWIP_Process();

HAL_GPIO_WritePin(GPIOB, GPIO_PIN_0, GreenLED);
HAL_GPIO_WritePin(GPIOB, GPIO_PIN_7, BlueLED);
HAL_GPIO_WritePin(GPIOB, GPIO_PIN_14, RedLED);
```

---

## Features

* Browser-controlled LEDs
* Embedded HTTP Server
* CGI support
* LwIP integration
* Ethernet communication

---

## Skills Learned

* Embedded Web Servers
* HTTP Protocol
* CGI Programming
* STM32 HAL Drivers
* LwIP Stack
* Ethernet Networking

---

# Task 2 – Embedded Web Server using STM32F446RCTx and W5500

## Overview

Task 2 involved implementing a standalone Embedded Web Server using a custom board containing an STM32F446RCTx microcontroller and a WIZnet W5500 Ethernet controller connected through SPI.

Unlike Task 1, which uses LwIP software TCP/IP stack, this project uses the W5500 hardware TCP/IP engine.

---

## Objectives

* Verify UART communication.
* Establish SPI communication with W5500.
* Configure Ethernet network settings.
* Perform Ping tests.
* Host HTML webpages using HTTP.

---

## Hardware Used

| Component           | Purpose              |
| ------------------- | -------------------- |
| STM32F446RCTx       | Main Controller      |
| W5500 Ethernet Chip | TCP/IP Communication |
| RJ45 Connector      | Ethernet Interface   |
| Router              | Network              |
| PC                  | Browser Access       |

---

## Project Stages

### Stage 1 – UART Test

Purpose:

* Verify serial communication.

Expected output:

```text
UART3 Working
```

---

### Stage 2 – SPI Test

Purpose:

* Verify communication between STM32 and W5500.

Expected output:

```text
W5500 Version = 0x04
```

---

### Stage 3 – Network Ping Test

Command:

```bash
ping 192.168.1.100
```

Expected:

```text
Reply from 192.168.1.100
```

---

### Stage 4 – HTTP Server

Open:

```text
http://192.168.1.100
```

The webpage served by STM32 appears in the browser.

---

## CubeMX Configuration

### SPI2

| Pin  | Function |
| ---- | -------- |
| PB13 | SCK      |
| PB14 | MISO     |
| PB15 | MOSI     |

### GPIO

| Pin  | Purpose     |
| ---- | ----------- |
| PA6  | W5500 Reset |
| PB12 | Chip Select |

### USART3

* Baud Rate = 115200
* TX/RX enabled

---

## Network Configuration

```c
MAC = 00:08:DC:11:22:33
IP = 192.168.1.100
Subnet = 255.255.255.0
Gateway = 192.168.1.1
```

---

## Socket State Machine

### SOCK_CLOSED

* Open socket
* Start listening

### SOCK_ESTABLISHED

* Receive browser request
* Send HTTP header
* Send webpage

### SOCK_CLOSE_WAIT

* Disconnect socket

---

## Source Files

### main.c

Contains:

* UART redirection
* SPI callback functions
* W5500 initialization
* Socket management
* Network configuration

### webpage.h

Stores webpage data.

### w5500.c / w5500.h

W5500 driver functions.

### socket.c / socket.h

TCP socket APIs.

### wizchip_conf.c / wizchip_conf.h

WIZnet configuration APIs.

---

## Features

* Hardware TCP/IP
* Embedded HTTP Server
* UART Debugging
* Static IP Configuration
* Bare-Metal Operation

---

## Skills Learned

* SPI Communication
* Socket Programming
* TCP/IP Fundamentals
* Embedded Debugging
* WIZnet ioLibrary

---

# Task 3 – HTML to C Byte Array Conversion using Perl

## Overview

Task 3 extends Task 2 by enabling larger and cleaner webpages.

Instead of storing HTML directly inside main.c, a Perl script converts HTML files into C arrays stored in Flash memory.

This eliminates the need for a filesystem or operating system.

---

## Objectives

* Convert HTML into C arrays.
* Generate webpage.c and webpage.h.
* Store webpages in Flash memory.
* Improve firmware maintainability.

---

## Workflow

### Step 1

Create:

```text
index.html
```

### Step 2

Create:

```text
html2c.pl
```

### Step 3

Run:

```bash
perl html2c.pl
```

### Step 4

Generated files:

```text
webpage.c
webpage.h
```

### Step 5

Copy files to:

```text
Core/Src
Core/Inc
```

### Step 6

Build and flash firmware.

---

## Generated Files

### webpage.c

Contains:

```c
const unsigned char index_html[];
const unsigned int index_html_len;
```

### webpage.h

Contains:

```c
extern const unsigned char index_html[];
extern const unsigned int index_html_len;
```

---

## Usage in main.c

```c
send(socket_num,
     (uint8_t*)index_html,
     index_html_len);
```

---

## Advantages

### Cleaner Code

Instead of:

```c
const char html[] = "...";
```

Use:

```text
webpage.c
webpage.h
```

### Easy Maintenance

Edit:

```text
index.html
```

Run:

```bash
perl html2c.pl
```

Rebuild the project.

### Efficient Flash Usage

* No filesystem required
* Entire webpage stored inside Flash
* Suitable for bare-metal systems

---

## Features

* HTML-to-C conversion
* Flash memory storage
* Improved maintainability
* Browser compatibility
* No RTOS required

---

## Skills Learned

* Perl Scripting
* Memory Management
* Flash Storage
* Embedded Web Design

---

# Overall Learning Outcomes

During this internship project, I gained practical experience in:

* Embedded C Programming
* STM32 HAL Drivers
* STM32CubeIDE
* STM32CubeMX
* Ethernet Communication
* SPI Protocol
* UART Debugging
* HTTP Protocol
* TCP/IP Networking
* WIZnet W5500
* LwIP Middleware
* CGI Programming
* Socket Programming
* Perl Scripting
* Flash Memory Management
* Embedded Web Server Development

---

# Internship Details

**Company:** RDL Technologies Ltd.
**Location:** Adyar, Mangalore
**Domain:** Embedded Systems and IoT

---

## Author

**Name:** Your Name
**Internship:** RDL Technologies Ltd.
**Year:** 2026

```
```
