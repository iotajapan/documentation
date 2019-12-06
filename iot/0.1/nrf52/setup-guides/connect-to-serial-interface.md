# Connect to the microcontroller's serial interface

**To read the output of your microcontroller, you need to be able to receive and transmit data on its serial port through a USB-to-UART connector. Some microcontroller development boards include an integrated USB-to-UART connector, but others do not. In this guide, you connect an external USB-to-UART connector to your microcontroller and plug it into your PC to be able to communicate with the microcontroller.**

## Hardware

To complete this guide, you need DuPont cables to connect the USB-to-UART connector to your microcontroller

## Step 1. Choose a USB-to-UART connector

Many USB-to-UART connectors are available, and you may find that they are named differently, depending on the vendor. For example, you may see USB-to-TTL and USB-to-serial connectors.

Whichever connector you choose, make sure that it has a [device driver](https://en.wikipedia.org/wiki/Device_driver) that's compatible with your operating system.

## Step 2. Wire the connector to your microcontroller

To use your connector, you need to wire it to your microcontroller.

1. Connect the following pins from the nRF52 to the connector

    |    **nRF52 (pin)**   |    **USB-to-UART**  |
    |------------------|------------------|
    |    TX (P0.6)     |    RX            |
    |    RX (P0.8)     |    TX            |
    |    VCC           |    VCC           |
    |    GND           |    GND           |

    **USB-to-UART**

    :::info:
    Use a black DuPont cable for GND and a red one for VCC
    :::

2. Plug the UART-to-USB dongle into your PC

If the connections are correct, your PC should detect the connector.

## Next steps

[Continue setting up your microcontroller](../introduction/get-started.md#step-2-set-up-your-development-environment).
