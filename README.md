SPI Protocol Verification using SystemVerilog
This repository contains a SystemVerilog verification environment designed to test an SPI (Serial Peripheral Interface) Master-Slave Core. The testbench uses a modular, Object-Oriented Programming (OOP) architecture where components communicate using Mailboxes and Events.

Project Structure
• SPI Master (Design): Converts 12-bit parallel input data into serial data on the MOSI line when the new data flag is high. It generates the serial clock using a frequency divider.
• SPI Slave (Design): Monitors the chip select line, reads serial data from the MOSI line, shifts it into a 12-bit register, and asserts a done flag when the transmission finishes.
• Transaction (Testbench): The basic data object that holds the randomizable 12-bit data and status variables.
• Generator (Testbench): Randomizes data packets and sends them to the driver while waiting for handshake signals.
• Driver (Testbench): Receives transactions from the generator, controls the virtual interface lines, and handles the reset sequence.
• Monitor (Testbench): Samples the output data and status signals directly from the interface pins.
• Scoreboard (Testbench): Compares the original data sent by the driver with the output data captured by the monitor to check for mismatches.
• Environment (Testbench): Instantiates all components, connects the mailboxes, and runs the simulation phases.

How to Run
You can simulate this project using platforms like EDA Playground or command-line simulators like QuestaSim or VCS.
Steps to compile and run using a standard simulator command line:
bash
vlog spi_project.sv
vsim tb -do "run -all; quit"
Use code with caution.

Sample Output Log
text
[DRV]: Reset done
[GEN]: din: 2451
[SCO] Data rcvd from MON: 2451, DRV: 2451
[SCO]: Data matched
[GEN]: din: 3892
[SCO] Data rcvd from MON: 3892, DRV: 3892
[SCO]: Data matched
