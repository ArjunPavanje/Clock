# Digital Clock

A fully functional **HH:MM:SS digital clock** implemented on the **ATmega328P (Arduino Uno)** using **AVR Assembly language** and **six 7-segment displays**.

The primary goal of this project was to maximize timing accuracy while minimizing hardware usage. Unlike conventional implementations that use one decoder per display and high-level increment operations, this clock:

* Uses **only one 7447 BCD-to-7-segment decoder** for all six displays.
* Implements **time incrementation using Karnaugh map (K-map) derived logic** instead of `INC` instructions.
* Is written entirely in **AVR Assembly**, providing precise control over instruction execution and timing behavior.

The project aims to demonstrate how low-level software optimization and hardware multiplexing techniques can significantly reduce component count while maintaining accurate timekeeping.

---

## Hardware Requirements

| Quantity | Component                       |
| -------- | ------------------------------- |
| 1        | Arduino Uno        |
| 6        | 7-Segment Displays |
| 1        | 7447 BCD-to-7-Segment Decoder   |
| -        | Breadboard                      |
| -        | Jumper Wires                    |
| -        | Resistors                       |

---

## Hardware Design

The six displays share a single 7447 decoder through a multiplexing scheme.

* The decoder outputs are connected in parallel to all displays.
* Each display's common pin is connected to a separate Arduino digital pin.
* Only one display is enabled at a time.
* The microcontroller rapidly cycles through all six displays.

Although only one display is active at any instant, the refresh rate is high enough that the human eye perceives all six displays as being continuously illuminated.

---

## K-map Based Time Increment Logic

Instead of using the AVR `INC` instruction, every decimal digit increment is implemented using Boolean expressions derived from Karnaugh maps. Separate K-map implementations are used for each digit.

---

## Building

### Assemble

```bash
avra main.asm
```

This generates:

```text
main.hex
main.obj
main.eep.hex
```

### Upload to Arduino Uno

Using `avrdude`:

```bash
avrdude -p atmega328p -c arduino -P /dev/ttyACM0 -U flash:w:main.hex
```

On Linux, the port may be:

```text
/dev/ttyUSB0
/dev/ttyACM0
```

Check available ports using:

```bash
ls /dev/tty*
```

---       
