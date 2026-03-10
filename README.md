# RISC-V-Processor-AHB-SoC-and-Multiple-peripherals-using-AHB-Bus
# RISC-V-32I-5-Stage-Pipeline-Core
**5 stage pipeline implementation of RISC-V 32I Processor.**

In this repository I have implemented 5 stage Pipelined processor which is actually the conversion of my previous single cycle implementation of processor into pipeline.


In case of pipelined implementation what we do is that we divide our instruction into multiple stages and in case of 5 stage pipelined implementation we will offcourse divide the instruction into 5 different stages. Five different stages are given as:
- Instruction Fetch
- Instruction Decode
- Instruction Execution
- Memory Read/Write
- Write Back

This pipelined implementation of processor supports six basic instructions:
- R-Type
- I-Type
- S-Type
- B-Type
- J-Type
- U-Type

This RISC V 5 Stage pipeline Implementation does encounters hazards, and it has been surpassed by implementing a hazard unit to handle all types of hazards(Structural and Data Hazard). 

# Implementation and Procedure 

5 Stage pipeline requires a series of registers between the complete datapath, these registers will be responsible for tracking of instruction or different partss of instructions required by different modules. The instructions needs to be propogated into all five stages for the instruction to be executed correctly and with the help of these registers the corresponding instructions propogate or different parts of instructions accordingly. The datapath followed is mentioned below and is the extened version of same implemented single cycle datapath as tagged above. 

![block diagram](https://github.com/user-attachments/assets/42f3b097-bc33-449c-b613-3b0cdca62cc9)

# Discussions

 **1. Fetch Cycle Datapath**
 
The Fetch cycle is the first stage of instruction execution process. The main goal of the fetch cycle is to retrieve the next instruction from memory so        that it can be decoded and executed by the processor.
     
**2. Decode Cycle Datapath** 

The decode cycle in a is the second stage of instruction execution. The main objective of this stage is to interpret       the fetched instruction and prepare      the necessary inputs (registers, control signals) for subsequent stages.

**3. Execution Cycle Datapath**

The Execution cycle is the third stage of instruction execution process. Its main role is to perform the arithmetic or logical   operation dictated by the          instruction, calculate memory addresses for load/store operations, or determine the outcome of a branch.
    
**4. Memory Read/Write Cycle Datapath**

The Memory Read or Write Cycle is the fourth stage of instruction execution process. This stage is responsible for interacting with data memory during load     or store instructions. If the instruction is not a memory operation, this stage is skipped, and the processor moves to the writeback stage.

**5. Write Back Cycle Datapath**

The Writeback cycle is the fifth and final stage of instruction execution process. The main purpose of this stage is to write the result of an instruction (whether it be from an arithmetic operation or a memory load) back to the destination register.

**Note: Hazard Unit**

**Hazard units** in a pipeline processor are responsible for detecting and resolving hazards that can occur when executing instructions in a pipelined         
architecture. 
**Hazards** can cause incorrect program execution or reduce performance by stalling the pipeline. 
There are three primary types of hazards: **data hazards, control hazards, and structural hazards**. 
In a 32-bit RISC-V 5-stage pipeline, **hazard detection and forwarding units** are critical components that help manage these hazards.

**Structural Hazard**
1. Hardware does not support the execution of instruction in same clock cycle.
2. Without having Two memories RISC-V pipelining architecture will have structural hazard.

**Data Hazard**
1. Data to be executed is not available.
2. May occur when pipeline is stalled.
3. Solve by using forwarding or bypassing technique.

**Solution to Data Hazard.**
1.Solving Data Hazards with nops
2.Solving Data Hazard with Forwarding / Bypassing

The Data Hazard is solved using Forwarding/ Bypassing

**Conditiion Table:**
![Screenshot 2024-09-06 203221](https://github.com/user-attachments/assets/df426216-b65d-4fca-a8b4-4416f27851c8)

**Condition for Data Hazard:**

![Screenshot 2024-09-06 203244](https://github.com/user-attachments/assets/1350b3e3-edb7-40ee-bf39-dd62a56339e2)

**Hazard Architecture:**

![Screenshot 2024-09-06 203309](https://github.com/user-attachments/assets/4ff4a805-0982-40df-81b8-b5d112d0e71e)

**Hazard Unit Waveform Explanation**

![hazard exp](https://github.com/user-attachments/assets/65d8acc2-d5a8-4342-bfd3-aba943781752)

**Note: The circled registers represents hazard unit**

# Simulation Results and Tools Used

The simulation has been in **Visual Studio** code with **Icarus Verilog environment**, which supports **gtkwave** for verilog simulaiton.

**The Input Machine Learning Codes are present inside the mem.hexfile.**

**Pipeline Simulation Waveform:**

![pipeline waveform](https://github.com/user-attachments/assets/a2ff6866-87fa-4aa0-b489-59c82bb6be5b)

**Note:** Download **Icarus Verilog** from:https://bleyer.org/icarus, after downloading in **CMD** type **iverilog**, you can see iverilog with gtkwave preinstalled.

**Terminal code:**

Eg: PS C:\Users\user\Desktop\Github projects\RISC_V_Single_Cycle_Core\Load Word I Type> **iverilog -o out.vvp .\pipeline_tb.v .\Pipeline_Top.v,**
**vvp out.vvp,**
**gtkwave.**

<dr>
# Multi-layer-AHB-lite
# AHB-Lite Based SoC Design

## 📌 Overview
This project demonstrates the incremental development of an AMBA AHB-Lite based system-on-chip (SoC), starting from a **single-layer AHB-Lite system**, extending to a **multi-layer AHB-Lite interconnect**, and finally integrating a **RISC-V pipelined processor** as the host (master) in the multi-layer system.

The design connects to three memory-mapped peripherals (slaves):
- **Timer**
- **GPIO**
- **Register File**

---

## 🚀 Design Flow

### 1. Single-Layer AHB-Lite System
- Architecture includes **one master** and **multiple slaves**.
- Master initiates read/write transactions.
- Address decoding logic routes transactions to:
  - Timer
  - GPIO
  - Register File

---

### 2. Multi-Layer AHB-Lite System
- Extended to support **multiple masters** with parallelism.
- Key components of the interconnect:
  - **Arbiter** – resolves conflicts between masters.
  - **Slave Interface** – manages communication with slaves.
  - **Multiplexer** – routes master-to-slave signals.
- Each slave instantiates its own set of interconnect blocks.
- Supports **parallel transactions** when masters access different slaves.

---

### 3. RISC-V Integration
- A **RISC-V pipelined processor** is integrated as **Host A (Master A)**.
- A **testbench acts as Host B (Master B)**.
- Both masters connect to the multi-layer AHB-Lite interconnect.
- The RISC-V processor is customized to be compatible with the SoC:
  - Data memory is extended for memory-mapped peripherals.
  - Address mapping:
    - `0x0000 – 0x001F` → GPIO
    - `0x0020 – 0x003F` → Timer
    - `0x0040 and above` → Register File
   
### Work Idea:
- The **RISC APB Wrapper** takes the instruction from the **RISC-V processor** and checks if it is a load word (lw) or store word (sw) instruction with an offset greater than 1000. If so, it stops the processor by freezing the PC counter and routes the instruction through one of the peripherals (e.g., UART).
- The **RISC APB Wrapper** then translates the RISC-V instructions into specific instructions that can be understood by the standard **APB Master bus**.
- The **APB bus** generates the APB signals according to the input signals from the **RISC APB Wrapper** and sends these signals to the **APB Decoder**.
- The **APB Decoder** selects the appropriate peripheral based on the instruction's address offset:
  - If the offset is greater than 1000 and less than 2000, it chooses **Peripheral 1 (UART)**.
  - If the offset is greater than 2000 and less than 3000, it chooses **Peripheral 2**.
  - If the offset is greater than 3000 and less than 4000, it chooses **Peripheral 3**.
- The **APB Decoder** also converts the APB signals to peripheral-specific signals and converts the peripheral signals (e.g., **Ready** signal) back to APB signals to be sent to the APB Master.

> **Note:** If you want a deeper look into each component separately, check the following:

> - [APB Protocol](https://github.com/Sai2521/AMBA_AHB_Lite_with_two_slaves)
> - [UART Peripheral](https://github.com/Sai2521/UART_FIFO/blob/main/README.md)


---

## Primary Signals

| **Signal**           | **Direction** | **Description**                                                                                               |
|----------------------|---------------|---------------------------------------------------------------------------------------------------------------|
| **clk**              | Input         | System clock driving the entire SoC.                                                                          |
| **rst**              | Input         | Resets the SoC logic and peripherals.                                                                         |
| **data_out**         | Output        | Parallel output data from the UART module, increased by one bit (9 bits).                                      |
| **OE**               | Output        | Output error signal for Overrun Error (OE) from UART module.                                                   |
| **BE**               | Output        | Output error signal for Break Error (BE) from UART module.                                                     |
| **FE**               | Output        | Output error signal for Framing Error (FE) from UART module.                                                   |
| **cancel_data_memory** | Wire        | Signal to cancel using data memory in case of `lw` or `sw` in a peripheral.                                    |
| **stop**             | Wire          | Stops the RISC-V PC from incrementing until data is sent or received using the APB bus.                        |
| **Instr**            | Wire          | 32-bit instruction from RISC-V processor.                                                                     |
| **Reg1_out**         | Wire          | 32-bit output wire from RD1 in the register file.                                                             |
| **Reg2_out**         | Wire          | 32-bit output wire from RD2 in the register file.                                                             |
| **READY**            | Wire          | Signal to indicate readiness (high when back to the normal RISC-V cycle).                                      |
| **SLVERR**           | Wire          | Indicates an error signal in the APB transaction.                                                             |
| **transfer**         | Wire          | Signal to start using the APB master.                                                                         |
| **SWRITE**           | Wire          | Write signal for the APB master to indicate a write transaction.                                               |
| **SADDR**            | Wire          | 32-bit address for the APB transaction.                                                                       |
| **SWDATA**           | Wire          | 32-bit write data for the APB transaction.                                                                    |
| **SSTRB**            | Wire          | 4-bit write strobes for the APB transaction (indicates which bytes are active).                                |
| **PSEL**             | Wire          | Peripheral select signal to choose the target peripheral in the APB transaction.                               |
| **PENABLE**          | Wire          | Enable signal for the APB transaction (indicates the second phase of the APB protocol).                        |
| **PWRITE**           | Wire          | Write signal to indicate whether the current APB transaction is a write.                                       |
| **PADDR**            | Wire          | 32-bit address sent to the peripheral in the APB transaction.                                                 |
| **PWDATA**           | Wire          | 32-bit write data sent to the peripheral in the APB transaction.                                              |
| **PSTRB**            | Wire          | 4-bit write strobes sent to the peripheral in the APB transaction.                                            |
| **PREADY**           | Wire          | Ready signal from the peripheral (high when the peripheral is ready to complete the transaction).              |
| **PSLVERR**          | Wire          | Error signal from the peripheral (indicates if there is a failure in the transaction).                         |
| **PREADY_W**         | Wire          | Ready signal for write transactions from the UART peripheral.                                                  |
| **PREADY_R**         | Wire          | Ready signal for read transactions from the UART peripheral.                                                   |
| **PUARTERR**         | Wire          | Error signal from the UART peripheral.                                                                        |
| **Rx_ready_APB**      | Wire         | Flag to indicate that the Rx FIFO is ready for receiving data from the Tx FIFO in the UART peripheral.         |
| **start_Tx**         | Wire          | Start signal for transmission (active low), controlled by `Rx_ready_APB`.                                      |
| **write_uart**       | Wire          | Write signal for the UART peripheral.                                                                         |
| **read_uart**        | Wire          | Read signal for the UART peripheral.                                                                          |
| **parity_sel**       | Wire          | Parity selection signal for the UART module.                                                                  |
| **baud_selector**    | Wire          | 2-bit signal for selecting the baud rate of the UART peripheral.                                              |
| **data_out_to_uart** | Wire          | 8-bit data output to the UART peripheral for transmission.                                                    |
| **new_instruction_Tx** | Wire       | Flag indicating that the upcoming instruction is new for the Tx FIFO, used to reset the serial counter.        |
| **new_instruction_Rx** | Wire       | Flag indicating that the upcoming instruction is new for the Rx FIFO, used to reset the serial counter.        |
