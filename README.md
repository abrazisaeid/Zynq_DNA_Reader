# Zynq-7020 Device DNA Reader IPCore (AXI-Lite Peripheral)

This project implements a custom AXI-Lite IP core in Verilog to read the unique 57-bit Device DNA from a Xilinx Zynq-7020 FPGA. The DNA value is accessible via AXI registers, making it easy to integrate into software running on the ARM processing system (PS) or external logic.

## 🔧 Features

- Uses Xilinx primitive `DNA_PORT` to access the unique hardware identifier.
- Fully compatible with AXI4-Lite interface.
- 57-bit DNA value is shifted out and stored in a 64-bit register (only lower 57 bits are valid).
- Clean FSM-based control logic (with states: RESET, READ_DNA, DONE).
- Easily extendable for security, licensing, or authentication purposes.

## 📦 IP Core Structure

- **FSM Logic**: Handles `READ` and `SHIFT` control signals for the `DNA_PORT`.
- **AXI Interface**: Exposes the DNA value through two 32-bit registers:
  - `0x00`: lower 32 bits of DNA
  - `0x04`: upper bits (bits 56:32)
- DNA bits are captured LSB-first and assembled into a 64-bit register (`SRL_O`).

## 🚀 How It Works

1. Assert `READ` and `SHIFT` for one clock cycle to initialize the read.
2. Keep `SHIFT=1` and shift the 57 bits serially from `DOUT`.
3. Capture each bit into a shift register on the rising edge of the clock.
4. When done, the DNA is available on `SRL_O[56:0]`.

📁 File Overview
zynq_AXI_DNA_v1_0_S00_AXI.v: Main AXI IP implementation

DNA_PORT: Xilinx primitive (instantiated directly)

SRL_O: Output register containing the 57-bit DNA

✅ Usage
Add this IP to your Vivado project using IP Packager.

Connect the AXI interface to Zynq PS or custom master.

Read SRL_O via AXI once the DONE state is reached.

Use it for device identification, licensing, or system authentication.

⚠️ Note
The DNA value is device-specific and permanently programmed by Xilinx. No two Zynq devices have the same DNA.

If you found this useful, feel free to ⭐ star this repo or contribute!
