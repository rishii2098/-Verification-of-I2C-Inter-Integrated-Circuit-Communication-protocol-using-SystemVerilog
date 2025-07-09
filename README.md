# I2C Master-Slave SystemVerilog Verification

This project implements and verifies a custom **I2C Master-Slave protocol** using SystemVerilog with a layered UVM-style testbench. It supports **read** and **write** transactions with randomized input and scoreboard-based checking.

🔗 **Live Simulation:**  
[![EDA Playground](https://img.shields.io/badge/Run%20on-EDA%20Playground-orange)](https://edaplayground.com/x/n6ys)

---

## 🧠 Design Summary

### 🔹 `i2c_master`
- Generates START, STOP, and clock pulses (SCL) from a 40 MHz system clock.
- Supports both **read** and **write** operations based on `op` signal.
- Handles data transmission over `sda` and checks ACK from slave.
- FSM with stages: `idle → start → write_addr → ack → write_data/read_data → stop`.

### 🔹 `i2c_slave`
- Responds to master's START condition.
- Decodes address and operation type.
- Performs memory read or write to internal memory (`mem[128]`).
- Acknowledges reception and handles STOP condition.

### 🔹 `i2c_top`
- Connects Master and Slave via bidirectional `sda` and shared `scl`.

---

## 🧪 Testbench Summary

### 🌐 Interface: `i2c_if`
Connects DUT to testbench and passes:
- `clk`, `rst`, `newd`, `op`, `addr`, `din`, `dout`, `busy`, `done`, `ack_err`

### 📦 `transaction`
- Defines stimulus with fields:
  - `op` (read/write)
  - `addr` (7-bit)
  - `din`, `dout`
- Random constraints applied.

### 🧬 `generator`
- Randomizes `transaction` and sends to `driver`.

### 🛠️ `driver`
- Applies transactions via the interface.
- Waits for `done` from DUT and synchronizes with `monitor`.

### 📡 `monitor`
- Captures DUT output into transaction format.
- Sends output to scoreboard for checking.

### 🧾 `scoreboard`
- Maintains a shadow memory.
- On write: updates memory.
- On read: checks if `dout == expected`.

---

## ✅ Features

- ✅ Full-duplex Master-Slave I2C protocol simulation
- ✅ START, STOP, ACK/NACK logic handled
- ✅ Randomized functional testbench
- ✅ Internal memory verification via scoreboard
- ✅ Events for sync (`drvnext`, `sconext`)

---

