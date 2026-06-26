# pcpower-esphome

ESPHome configuration for PCPower boards, allowing easy updating, flashing, and control of computers via front panel connectors.

Board link WIP, may be provided as a schematic or standalone pre-made boards, but easily created with an ESP8266 (Wemos D1 Mini w/ ESP8266)

## Creating a board

The boards are very simple and can be made with components off Amazon/other sites.

The resistors are used simply to limit current to the optocouplers, with two optocouplers being outputs (Power/Reset) and one being input (Power LED).

The Power LED of the case is then driven off the board's output.

| Material / Component | Specifications / Value | Quantity | Link |
| :--- | :--- | :--- | :--- |
| **ESP8266 Microcontroller** | NodeMCU / Wemos D1 Mini board variant | 1 | [Amazon](https://www.amazon.com/Organizer-ESP8266-Internet-Development-Compatible/dp/B081PX9YFV) |
| **Resistors** | 220 $\Omega$ (Ohm) | 3 | [Amazon](https://www.amazon.com/dp/B07HDGF48W) |
| **P817 Optocouplers** | PC817 / P817 DIP-4 package | 3 | [Amazon](https://www.amazon.com/dp/B01JG8EJVW) |

| Script GPIO Number | Wemos Board Label | Physical Component Connected | Purpose / Behavior |
| :---: | :---: | :--- | :--- |
| **GPIO5** | **D1** | **Power LED Input** (From Optocoupler) | Monitors the PC's power state via Power LED output |
| **GPIO12** | **D6** | **Optocoupler (Power)** (Force Power) | Linked to D7, can be used for more control over firmware (Tasmota, etc) |
| **GPIO13** | **D7** | **Optocoupler (Power)** (Standard Power) | Toggles the standard motherboard power switch. |
| **GPIO14** | **D5** | **Status LED Output** | Connects directly to the case power LED. |
| **GPIO15** | **D8** | **Reset Optocoupler** | Toggles the standard motherboard reset switch. |

## GL.iNet KVM

The ESPHome configuration allows Comet KVM devices to access the device over serial. The device can be plugged into the USB 2.0 port just like the official power control board.

To enable support on the Comet KVM devices, copy the `99-wemos-alias.rules` file into /etc/udev/rules.d/ on the device.

Full instructions:

- Access the terminal (Toolbox -> Access Terminal)
- Download the rules file (`wget -O /etc/udev/rules.d/99-wemos-alias.rules https://raw.githubusercontent.com/tystuyfzand/pcpower-esphome/refs/heads/main/99-wemos-alias.rules`)
- Reload udev (`udevadm control --reload-rules`)
- Force triggers (`udevadm trigger`)
