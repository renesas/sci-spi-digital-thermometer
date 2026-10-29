# SCI SPI Digital Thermometer

## Overview

This software building block (SBB) demonstrates basic use of the SCI SPI driver with an Analog Devices Maxim Integrated MAX31723PMB1 temperature sensor module.

The SCI SPI interface operates as the master. The reference application writes commands to the temperature sensor, reads the temperature value, and continuously prints the temperature readings to a serial terminal.

## Supported Boards

This document covers only the following boards:

- FPB-RA2E3
- FPB-RA4E2
- FPB-RA6E2
- FPB-RA8E1

## What Use Case Does This SBB Demonstrate?

The SBB demonstrates how to:

1. Configure an SCI peripheral for SPI Master operation.
2. Send commands to a MAX31723PMB1 temperature sensor module.
3. Read temperature data from the sensor over the SCI SPI bus.
4. Convert or format the returned value for display.
5. Continuously print temperature readings to a serial terminal.

## What Does the SBB Core Code Do?

Based on the behavior described in the source README, the SBB coordinates communication between an RA MCU and the MAX31723PMB1 sensor through an SCI SPI Master interface.

The source `readme.txt` does not identify the implementation filenames, public APIs, callbacks, command values, register sequence, data conversion logic, transfer sizes, or error-handling behavior. These details must be documented from the source and header files before release.

## Reusable SBB Code and Reference Application Code

### Reusable SBB code

The reusable portion should contain device communication and data-handling logic that is independent of a particular FPB board, connector, terminal, or debug interface. This may include SCI SPI transfers, sensor command handling, reading temperature data, and reporting operation status through a defined interface.

### Reference application code

The following behavior is reference-application or demonstration behavior:

- Repeatedly reading the MAX31723PMB1 temperature value.
- Continuously printing temperature readings to a terminal.
- Selecting a board-specific SCI channel and SPI pins.
- Configuring PMOD routing, board option links, and jumpers.
- Using the J-Link On-Board VCOM interface for terminal output.

A product application may replace the continuous display loop and terminal output while retaining the reusable sensor communication logic.

## Public Functions to Review First

The source `readme.txt` does not list public function names. Before integration, identify and document the implementation functions responsible for:

1. Initializing the SBB and SCI SPI interface.
2. Initializing or configuring the MAX31723PMB1.
3. Starting an SPI write operation.
4. Starting an SPI read operation.
5. Reading and returning a temperature value.
6. Handling SCI SPI events or transfer completion.
7. Reporting communication errors.
8. Closing or resetting the SBB, if supported.

Use the actual function names exported by the delivered source and header files. Do not derive API names from this document.

## Hardware Requirements

- One supported Renesas RA FPB board.
- One USB cable suitable for programming and debugging the selected board.
- One MAX31723PMB1 temperature sensor module.
- Jumper wires where required to connect the sensor to the board.
- A host PC with a serial-terminal application, such as Tera Term, when terminal output is required.

## Board Configuration and Connections

### FPB-RA2E3

To use PMOD1 for SPI:

- Close E41 and E42.
- Open E35, E39, and E40.
- Connect the temperature sensor connector J1 to PMOD1.
- Carefully align pin 1 on the temperature sensor with pin 1 on PMOD1.

To use J-Link OB VCOM for the serial terminal:

- Close E25, E28, E46, and E47.

### FPB-RA4E2

To use PMOD1 for SPI:

- Close E3, E6, and E36.
- Open E4, E5, E35, and E37.
- Connect the temperature sensor connector J1 to PMOD1.
- Carefully align pin 1 on the temperature sensor with pin 1 on PMOD1.

### FPB-RA6E2

To use PMOD1 for SPI:

- Close E3, E6, and E36.
- Open E4, E5, E35, and E37.
- Connect the temperature sensor connector J1 to PMOD1.
- Carefully align pin 1 on the temperature sensor with pin 1 on PMOD1.

### FPB-RA8E1

To use PMOD2 for SPI:

- Close E32 and E33.
- Open E25 and E28.
- Connect the temperature sensor connector J1 to PMOD2.
- Carefully align pin 1 on the temperature sensor with pin 1 on PMOD2.

To use the on-board debug functionality:

- Open jumpers J6 and J9.


## Sensor Connection Precautions

- Align pin 1 of the MAX31723PMB1 module with pin 1 of the selected PMOD connector.
- Apply the board-specific option-link and jumper settings before operating the example.
- Verify power, ground, and signal routing against the selected board schematic and sensor module documentation before applying power.

The source `readme.txt` does not specify the detailed SPI signal-to-pin mapping for these FPB configurations.

## Serial Terminal Configuration

Use the serial port provided by the J-Link On-Board interface where available.

| Setting | Value |
|---|---|
| Speed | 115200 baud |
| Data | 8 bits |
| Parity | None |
| Stop bits | 1 |
| Flow control | None |

For Tera Term:

1. Open **Setup > Terminal**.
2. Under **New-line**, set both **Receive** and **Transmit** to **CR**.
3. Enable **Local echo** if typed characters should be echoed.

## GCC Float-Printing Configuration

When using the GCC ARM Embedded Toolchain, enable floating-point output for nano `printf`:

1. Open **Properties**.
2. Select **C/C++ Build > Settings**.
3. Select **Tool Settings > GNU Arm Cross C Linker > Miscellaneous**.
4. Enable **Use float with nano printf**.

This setting is required for printing floating-point temperature values to the terminal with nano `printf`.

## What Must Change for Another Board or Application?

### SPI peripheral and pins

- Select an SCI channel that supports SPI Master operation on the target MCU.
- Assign the required SPI signals through the FSP configuration.
- Configure the chip-select behavior required by the sensor and application.
- Update PMOD routing, option-link settings, jumper positions, and wiring instructions.

### Clocks and timing

- Verify the SCI peripheral clock and generated SPI bit rate.
- Confirm that the configured SPI mode and timing satisfy the MAX31723PMB1 requirements.
- Review transfer-completion and timeout handling in the implementation.

The source `readme.txt` does not state the SPI bit rate, SPI mode, clock polarity, clock phase, bit order, timeout values, or measured timing margins.

### Sensor handling

- Confirm the command and register sequence used to configure the sensor.
- Verify how raw sensor data is converted into a temperature value.
- Define the required sampling interval and error behavior for the product application.

These details are not provided in the source `readme.txt` and must be obtained from the implementation and sensor documentation.

### Terminal output

- Update the UART or VCOM configuration for the target board.
- Retain floating-point `printf` support when the application prints floating-point values.
- Replace or remove terminal output if it is not required by the product application.

### Memory, task priorities, and middleware

The source `readme.txt` does not specify memory usage, RTOS requirements, task priorities, stack sizes, DMA or transfer-driver usage, or middleware configuration. Review these areas in the implementation when integrating the SBB into a larger application.

## Dependencies

The demonstrated project depends on:

- Renesas Flexible Software Package configuration for SCI SPI Master operation.
- A MAX31723PMB1 temperature sensor module.
- Board-specific SPI pin and PMOD routing.
- A USB debug connection.
- A serial output path and terminal application.
- Floating-point support in nano `printf` when using the GCC ARM Embedded Toolchain to display floating-point temperature values.

The source `readme.txt` does not provide the FSP version, exact driver module names, sensor driver structure, middleware versions, or generated instance names.

## Demonstrated Behavior and Performance

The documented reference behavior is:

- The SCI SPI bus operates as the master.
- The application writes commands to the MAX31723PMB1 module.
- The application reads the temperature value from the module.
- Temperature values are continuously printed to the terminal.

The source `readme.txt` does not provide the SPI frequency, sampling rate, temperature accuracy, test duration, number of samples, transfer error rate, CPU usage, or memory usage. The example therefore documents functional behavior, not quantified performance.

## File Ownership and Modification Guidance

The source `readme.txt` does not list the project files. Classify the delivered files before modification.

### Safe places for customer changes

- Customer application source files.
- Customer-owned board configuration.
- SBB wrapper or adaptation files.
- Clearly identified user-editable regions in generated integration files.

### Reusable SBB files

Keep sensor communication and temperature-reading logic separate from board routing, terminal presentation, and the continuous demonstration loop. Track any local changes made directly to reusable SBB files.

### Generated or preset files

Treat FSP-generated source, pin configuration, and configuration artifacts as generated unless the project explicitly identifies them as hand-maintained. Apply configuration changes through the supported FSP configuration source rather than by editing generated output directly.

### Demo-only files

Treat continuous terminal display, board-specific PMOD selection, and demonstration control flow as demo-only unless the product application explicitly adopts them.

## Updating to a Newer SBB Release After Local Modification

To reduce upgrade conflicts:

1. Keep released reusable SBB files unchanged where possible.
2. Put product-specific behavior in wrapper, adaptation, or application files.
3. Keep board configuration separate from sensor communication logic.
4. Record the adopted SBB version and all local modifications.
5. Compare the original release, locally modified version, and newer release during an upgrade.
6. Reapply only the required local changes.
7. Regenerate FSP-managed files using the FSP version required by the newer release.
8. Rebuild and repeat sensor communication, temperature display, and error-path tests on each supported target board.

Direct edits to reusable or generated files increase the effort required to adopt a newer SBB release.

## Expected Result

After the board is configured, the sensor is connected, and the application is programmed and started:

1. The SCI SPI Master sends commands to the MAX31723PMB1.
2. The application reads a temperature value from the sensor.
3. The temperature value is printed continuously on the serial terminal.

