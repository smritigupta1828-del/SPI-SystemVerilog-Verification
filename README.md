# SPI Protocol Verification Environment

This repository contains a modular, object-oriented testbench built in SystemVerilog to verify a standard SPI Master-Slave design. The testbench is organized using layered architecture principles to separate stimulus generation, pin-level driving, signal monitoring, and data checking.

## Testbench Architecture

The verification environment is broken down into separate class components that communicate using mailboxes and events:

* **SPI Master (Design):** Converts 12-bit parallel input data into serial data on the MOSI line when the new data flag is high. It generates the serial clock using an internal frequency divider.
* **SPI Slave (Design):** Monitors the chip select line, reads serial data from the MOSI line, shifts it into a 12-bit register, and asserts a done flag when the transmission finishes.
* **Transaction:** Defines the data fields for the SPI protocol, including the random input data (`din`) and the captured output data (`dout`). It includes a copy function to duplicate objects safely.
* **Generator:** Generates transaction objects, randomizes the input stimuli, and sends them to the driver. It uses a handshake event to wait until the scoreboard finishes checking the current transaction before generating the next one.
* **Driver:** Receives transactions from the generator through a mailbox. It handles the physical interface by running a reset sequence and then driving data onto the interface pins.
* **Monitor:** Observes the output pins of the SPI design through the virtual interface, captures the output state, and sends it to the scoreboard for evaluation.
* **Scoreboard:** Collects the captured transactions from the monitor and the reference transactions from the driver. It compares the two datasets to verify matching behavior and outputs error messages in case of mismatches.

## How to Run

You can simulate this project using platforms like EDA Playground or command-line simulators like QuestaSim or VCS.

Steps to compile and run using a standard simulator command line:

```bash
vlog spi_project.sv
vsim tb -do "run -all; quit"
```
## Sample Output log
[DRV]: Reset done
[GEN]: din: 2451
[SCO] Data rcvd from MON: 2451, DRV: 2451
[SCO]: Data matched
[GEN]: din: 3892
[SCO] Data rcvd from MON: 3892, DRV: 3892
[SCO]: Data matched
