# How an instruction executed?
----

## BASIC:-
----------

### 1. **When the System Starts (Boot Process)**
   - When the system is powered on or rebooted, the **bootloader** (a small program stored in non-volatile memory) is the first to run. The bootloader's job is to load the OS into memory and pass control to it.
   - The bootloader initializes the **PC** to the address of the first instruction of the OS kernel (the starting point of the OS code), which is typically located in a specific memory address.
   - For example, the **PC** could be initialized to an address like `0x0040` or another predefined memory address where the OS loader or first instruction is located.
   
### 2. **When a Program is Executed**
   - Once the OS is loaded and running, it manages the execution of user programs. 
   - When a program is launched, the **OS** loads the program's machine code (instructions) into memory. It sets the **PC** to the starting address of the program’s entry point (usually the first instruction of the program).
     - For example, if the program's first instruction is at address `0x0040`, the **OS** will initialize the **PC** to `0x0040` to start executing the program.
   
### 3. **During Program Execution**
   - After the **PC** is initialized, the CPU increments the **PC** automatically during each instruction cycle (after each instruction is fetched) to point to the next instruction in memory.

### **Summary:**
- **OS or Bootloader Initialization**: The **PC** is initialized by the OS (or bootloader) to the memory address where the first instruction of the program or OS is stored.
- **CPU Role**: After initialization, the CPU automatically increments and updates the **PC** to point to the next instruction during the execution cycle.

In essence, the **OS** ensures that the **PC** starts at the correct memory address when a program is launched or when the system boots up.
--------------------------------------------------

### **Step 1: Initializing the Program Counter (PC)**
The **Program Counter (PC)** is a special-purpose register in the CPU that stores the address of the **next instruction** to be executed. When a program is loaded into memory, the PC is initialized to point to the address of the **first instruction** in memory.

For example:
- If the first instruction of the program is loaded at memory address `0x0040`, the PC will be initialized to `0x0040`.

---

### **Step 2: Fetching the Instruction**
1. **PC Points to Memory:**
   - The **PC** holds the address of the next instruction to be executed.
   - Let's say the PC has the value `0x0040`, which points to the first instruction.
  
2. **Address Bus:**
   - The **PC** sends the address `0x0040` through the **Address Bus** to the memory.
   - The **Address Bus** is simply a set of wires that carry the binary address to memory.
   - For example, `0x0040` (which is `0000 0000 0100 0000` in binary) is transmitted through the Address Bus to the memory.

3. **Control Signals:**
   - Along with the address, the CPU sends a **READ** signal over the **Control Bus** to tell the memory to fetch data from the specified address.
   - The **Control Bus** includes various signals, but the **READ** signal specifies that the memory needs to read data (the instruction) from the address.

4. **Memory Access:**
   - Memory at address `0x0040` holds a machine-level instruction, for example:
     - Binary instruction: `1001101011000001`
   - The memory sends this instruction back to the CPU via the **Data Bus**.
   - The **Data Bus** is a set of wires that carries data (in this case, the binary instruction) from memory to the CPU.

---

### **Step 3: Decoding the Instruction**
Once the instruction is fetched, the CPU needs to decode it to understand what operation it should perform. The **Control Unit (CU)** in the CPU handles the decoding of the binary instruction.

1. **Control Unit (CU):**
   - The CU analyzes the fetched instruction (`1001101011000001`), breaking it down into its components.
   - The instruction has two parts: an **opcode** (operation code) and **operand(s)** (the data or register addresses involved in the operation).

   For example:
   - Opcode: `100110` (this could correspond to an **ADD** operation).
   - Operand: `1011000001` (this could refer to the registers or memory locations involved).

2. **Decoding Process:**
   - The CU decodes the instruction by identifying the operation (`ADD`) and the involved operands (e.g., registers or immediate values).
   - Based on the decoded opcode, the CU generates control signals to direct the **ALU**, **register file**, and other parts of the CPU on how to process the instruction.

---

### **Step 4: Executing the Instruction**
1. **ALU (Arithmetic Logic Unit):**
   - In the case of an **ADD** operation, the **ALU** performs the addition of two values.
   - For example, if the instruction is `ADD R1, R2, R3`, the ALU will add the values in registers `R1` and `R2` and store the result in `R3`.

2. **Registers:**
   - Registers hold the operands and results.
   - For example:
     - If `R1` = 5 and `R2` = 10, then `R3` will be assigned the value `R1 + R2` = 15 after the ALU operation.

3. **Execution of Other Operations:**
   - If the instruction is not an arithmetic operation (e.g., **LOAD**, **STORE**, or **BRANCH**), the ALU or other components (such as memory or the control unit) will perform those operations instead.

---

### **Step 5: Storing the Results**
After the instruction is executed, the CPU must store the results.

1. **Write Back to Register:**
   - If the instruction modifies a register (like the **ADD** operation), the result is written back to the specified register (`R3` in our example).
   
   - For example, after the addition operation, `R3` will hold the value 15.

2. **Write Back to Memory:**
   - If the instruction involves a memory operation (like **STORE**), the result will be written back to a memory location.
   - For example, the instruction `STORE R3, 0x0050` would store the value in register `R3` (which is 15) into memory address `0x0050`.

---

### **Step 6: Incrementing the Program Counter (PC)**
Once an instruction is executed and the results are stored, the **Program Counter (PC)** needs to be updated to point to the next instruction.

1. **PC Update:**
   - After executing the current instruction, the **PC** is incremented by the size of the instruction.
   - If the instruction is 4 bytes (32-bit), the PC will increment by 4.

   Example:
   - If the current value of the **PC** was `0x0040` and the instruction was 4 bytes long, the **PC** will be updated to `0x0044`.

2. **Repeat the Process:**
   - The CPU will then fetch the next instruction from the updated memory address (`0x0044`), and the cycle repeats.

---

### **Summarized Cycle (Fetch, Decode, Execute, Store Results):**
1. **Fetch**:
   - The PC sends the address to memory via the Address Bus, and the instruction is fetched from memory and placed on the Data Bus.
2. **Decode**:
   - The Control Unit decodes the instruction to determine the operation (e.g., ADD) and the operands (e.g., registers).
3. **Execute**:
   - The ALU or other units perform the specified operation (e.g., adding two registers).
4. **Store Results**:
   - The result is written back to the appropriate register or memory location.
5. **Increment PC**:
   - The PC is updated to point to the next instruction, and the process repeats.

---

### **Final Example (Simplified)**:
Let’s say the program starts at memory address `0x0040` with the instruction `ADD R1, R2, R3`:

1. The **PC** starts at `0x0040` and sends this address to memory via the Address Bus.
2. Memory at `0x0040` sends the instruction (`ADD R1, R2, R3`) back to the CPU via the Data Bus.
3. The **Control Unit** decodes the instruction and tells the ALU to add the contents of `R1` and `R2`, storing the result in `R3`.
4. The ALU performs the addition, and `R3` is updated with the result.
5. The **PC** is updated to `0x0044` to point to the next instruction.
6. The process repeats for the next instruction.

---

### In Summary:
- The **PC** keeps track of the address of the next instruction.
- The **Address Bus** carries the address from the CPU to memory.
- The **Control Unit** decodes instructions and sends control signals to direct the ALU and registers.
- The **ALU** performs the actual computation.
- The **Data Bus** carries the instruction from memory to the CPU and the result from the CPU back to memory or registers.
- After execution, the **PC** is incremented to fetch the next instruction, and the process continues.

This is how a CPU fetches, decodes, executes, and stores results while processing machine instructions at a high speed!
