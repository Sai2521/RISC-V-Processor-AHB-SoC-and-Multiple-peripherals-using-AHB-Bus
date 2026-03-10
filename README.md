# RISC-V-Processor-AHB-SoC-and-Multiple-peripherals-using-AHB-Bus
# **AHB-Lite Bus Implementation & Verification**

## Overview
This project implements an **AMBA AHB-Lite** system consisting of:
- A **Master**
- An **AHB Decoder**
- **Two Slaves** (with different coding styles and memory sizes)

![Our Implementation]![alt text](<ChatGPT Image Feb 13, 2026, 04_25_10 AM-1.png>)

The goal is to:
1. Understand and implement **AHB-Lite protocol fundamentals**
2. Verify functionality across different transfer types
3. Test **slave switching** via address decoding
4. Explore **different coding styles** for slave design (RTL vs FSM)

---

## About AHB-Lite
**AHB-Lite** is a simplified version of the AMBA AHB protocol, designed for systems with a **single master**.  
It is widely used in microcontrollers, SoCs, and embedded systems due to its:
- **High-performance pipelined bus** (supporting address and data phases in parallel)
- **Simple integration** for single-master designs
- Support for **different transfer types** (Single, Incrementing Burst, Wrapping Burst, etc.)
- Ability to **insert wait states** for slower slaves

---

## Data Transfer in AHB-Lite
Each AHB-Lite transfer consists of two **phases**:

- **Address Phase**  
  The master places the **address**, **control signals**, and **transfer type** on the bus.
  
- **Data Phase**  
  Data is transferred (read or write).  
  Pipelining allows the **next address** to be issued while the current data phase is still active, increasing throughput.

### **Common Transfer Types**
- **Single Transfer** (`BURST = 0`): One data transfer only.
- **Incrementing Burst** (`BURST = 1`): Multiple consecutive transfers, address increments after each.
- **Wrapping Burst**: Address wraps within a defined boundary.
- **Undefined Burst**: Burst length not predefined.

---

## Architecture
In this project, we compare:

1. **Original AHB-Lite Documentation Structure**  

   ![Original Architecture](https://github.com/MohamedHussein27/AMBA_AHB_Lite_with_two_slaves/blob/main/Strucutre%20%26%20Testing/General_Archeticture.png)

2. **Our Implementation**  

   ![Our Implementation](https://github.com/MohamedHussein27/AMBA_AHB_Lite_with_two_slaves/blob/main/Strucutre%20%26%20Testing/AHB_Lite_Archeticture.png)

### **Key Points in Our Implementation**
- **Processor Assumption**  
  An external processor drives the master signals, simulating a real-world scenario.
  
- **PDONE Signal**  
  Added to indicate a successful transaction from the master back to the processor.
  
- **Two-Slave Design**
  - **Slave 1**: Larger memory, **regular RTL coding style**
  - **Slave 2**: Smaller memory, **FSM coding style**, demonstrating multiple implementation approaches
  
- **AHB Decoder**  
  Directs transfers to the correct slave based on address.

---

## 🔍 Verification Flow
The verification strategy mimics a processor issuing transactions to test:
1. **Single Transfers** with no wait states
2. **Incrementing Bursts**
3. **Transfers to Slave 1**
4. **Transfers to Slave 2**
5. **Slave Switching** between transactions

---

### **Step 1 – Expected vs Actual Waveform: Single Transfer**
- **Expected**:
![Single Transfer Comparison](https://github.com/MohamedHussein27/AMBA_AHB_Lite_with_two_slaves/blob/main/Strucutre%20%26%20Testing/Expected_Single_Write.png)
- **Waveform**:

![Single Transfer Comparison](https://github.com/MohamedHussein27/AMBA_AHB_Lite_with_two_slaves/blob/main/Strucutre%20%26%20Testing/Generated_Single_Write.png)

In previous comparison we observe:
- In Write transfer: generated waveform matches the expected waveform in the AHB documentation
- In Write Transfer: HADDR appears at cycle N, HWDATA at cycle N+1 and data is accessed be the salve at cycle N+2 exactly like the specs stated!
- In Read Transfer: generated waveform matches the expected waveform in the AHB documentation
- In Read Transfer: HADDR appears at cycle N and HRDATA at cycle N+1 exactly like the specs stated!

---

### **Step 2 – Expected vs Actual Waveform: Incrementing Burst**
- **Expected**:
![Single Transfer Comparison](https://github.com/MohamedHussein27/AMBA_AHB_Lite_with_two_slaves/blob/main/Strucutre%20%26%20Testing/Expected_Burst_Transfer.png)
- **Writing Waveform**:

![Single Transfer Comparison](https://github.com/MohamedHussein27/AMBA_AHB_Lite_with_two_slaves/blob/main/Strucutre%20%26%20Testing/Generated_Burst_Write.png)
- **Reading Waveform**:

![Single Transfer Comparison](https://github.com/MohamedHussein27/AMBA_AHB_Lite_with_two_slaves/blob/main/Strucutre%20%26%20Testing/Generated_Burst_Read.png)

In previous comparison we observe:
-	In Write Transfer: generated waveform matches the expected waveform in the AHB documentation
-	In Write Transfer: HADDR appears at cycle N, HWDATA at cycle N+1 and data is accessed be the salve at cycle N+2 exactly like the specs stated!
-	As (HSIZE = 2’b01) this indicates that the transfer is 16 bit transfer so we notice that the address is incrementing by 2 every cycle!
-	In accessing memory, we see that we write 16 bits successfully in our memory in the N+3 cycle.

-	In Read Transfer: generated waveform matches the expected waveform in the AHB documentation
-	In Read Transfer: HADDR appears at cycle N and HRDATA at cycle N+1 exactly like the specs stated!
-	As (HSIZE = 2’b10) this indicates that the transfer is 32 bit transfer so we notice that the address is incrementing by 4 every cycle!
-	You may ask, why the data read didn’t change as we read from two different addresses?! but the answer is that we actually wrote the same data in these two addresses.

---

### **Step 3 – Slave 1 Verification**

![Slave 1 Verification](https://github.com/MohamedHussein27/AMBA_AHB_Lite_with_two_slaves/blob/main/Strucutre%20%26%20Testing/Slave1_Waveform.png)

---

### **Step 4 – Slave Switching Moment**

![Slave Switching](https://github.com/MohamedHussein27/AMBA_AHB_Lite_with_two_slaves/blob/main/Strucutre%20%26%20Testing/Switching%20Moment.png)

---

### **Step 5 – Slave 2 Verification**

![Slave 2 Verification](https://github.com/MohamedHussein27/AMBA_AHB_Lite_with_two_slaves/blob/main/Strucutre%20%26%20Testing/Slave2_Waveform.png)

---

## 📂 Additional Documentation
For better understanding and deeper visualization of the design and verification process,  
please refer to the [full documentation](https://github.com/MohamedHussein27/AMBA_AHB_Lite_with_two_slaves/blob/main/Documentation/AHB_Lite_Documentation.pdf) 

---

## FPGA Flow using Vivado
- **Elaboration**

![Elaboration](https://github.com/MohamedHussein27/AMBA_AHB_Lite_with_two_slaves/blob/main/Strucutre%20%26%20Testing/Elaboration.png)
- **Synthesis**

![Synthesis](https://github.com/MohamedHussein27/AMBA_AHB_Lite_with_two_slaves/blob/main/Strucutre%20%26%20Testing/Senthesis.png)

---

## ✅ Conclusion
- The design respects **address and data phase pipelining**
- Both **single** and **incrementing burst** transfers are implemented and verified
- The architecture allows for easy scaling to additional slaves or features
- Multiple coding styles for slaves were demonstrated

---

## Contact Me!
- [Email](mailto:Mohamed_Hussein2100924@outlook.com)
- [WhatsApp](https://wa.me/+2001097685797)
- [LinkedIn](https://www.linkedin.com/in/mohamed-hussein-274337231)
