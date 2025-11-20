# System Call

Controlled bridge that allows a low privilege application (Ring 3) to request services from the high privilege kernel (Ring 0).

## How it works?

1. **User application initiation (Ring 3)**

    User application calls a standard library function (like `read()`, `write()`, or `fork()`). This library function is often called a **wrapper function**. The wrapper function prepares the necessary parameters for the requested service and stores them in designated **CPU registers**. It also sets a **System Call Number** (index) in a specific register to tell the kernel which service is being requested.

2. **The privilege switch (The Trap)**

    The wrapper function executes a special low level CPU instruction. This instruction is the mechanism that triggers the switch. This instruction is usually `syscall`. While executing this instruction causes a **controlled exception (Trap)** that forces the CPU to switch its privilege level from Ring 3 to Ring 0. The CPU Hardware automatically saves the critical state of the user program onto a dedicated **kernel stack** so the program can correctly resume later.

3. **Kernel service execution (Ring 0)**

    Control is handed to the kernel **System Call Handler**. Kernel reads the ** System Call Number** from the register and uses it as an index into the **System Call Table** (an array of function pointers) to find and execute the correct kernel function (`sys_read`). Kernel must **validate** all arguments passed from the user program. Kernel function executes the privileged operation, such as directly communicating with the disk controller to retrieve data.

4. **Return to user space**

    When the kernel function completes its task, it prepares any **return values** (number of bytes read or success / error code) and loads them back into a designated CPU register. Kernel executes a final special instruction (`sysret`). This instruction restores the CPU state from the saved kernel stack, and switches the privilege level back from **Ring 0 to Ring 3**. The wrapper function receives the return value and then returns control to rest of the application, which resumes execution exactly where it left off, as if the privileged operation was just a regular function call.

## System Call Number (ID)

Is a **unique, small integer** assigned to every distinct service the kernel offers.

- **Role:** acts as a **unique identifier** or **index** for specific kernel function.
- **Mechanism:** when user space application (wrapper) wants a service (`read()` a file), it loads the corresponding System Call Number into designated **CPU Register**.
- **Example:** `__NR_exit` might be **1**, `__NR_fork` might be **2**, `__NR_read` might be **3**.

## System Call Table (Directory)

A critical data structure located in the kernel memory space. 

- **Role:** maps the System Call Number (index) to the actual memory address (function pointer) of the corresponding kernel service routine.
- **Structure:** **array of function pointers**, where the index of the array element is the System Call Number, and the value stored at that index is the address of the kernel function that implements that service.
- **Mechanism:** Kernel uses the System Call Number passed from the user process to look up the correct function address in this table. Example, if the number is 3, the kernel looks at index 3 in the table to find the memory address of the `sys_read()` function.

## System Call Handler (Gatekeeper / Router)

First piece of privileged code the CPU executes when a user process makes a system call.

- **Role:** **single entry point** into the kernel for all system calls. It manages the transition from the unprivileged **User Mode** to the privileged **Kernel Mode**.
- **Mechanism:**
    
    1. A special CPU instruction (like syscall or int 0x80) is executed in user mode, which causes a trap or software interrupt.
    
    2. The CPU hardware automatically switches the privilege level to Kernel Mode and transfers control to the System Call Handler.

    3. The Handler's first job is to save the state of the user process (registers, stack pointer, etc.) onto the kernel stack.

    4. It then reads the System Call Number from the designated CPU register.

    5. It uses this number to look up the actual service routine in the System Call Table.

    6. It validates the parameters passed by the user process for safety and permissions.

    7. Finally, it calls the appropriate kernel service routine (e.g., sys_read).

## Relationship

$$\text{User Process } \xrightarrow{\text{loads } \textbf{System Call Number}} \text{CPU Register} \xrightarrow{\text{trap}} \textbf{System Call Handler} \xrightarrow{\text{look up in } \textbf{System Call Table}} \text{Execute Kernel Function}$$

## Related

- **[Kernel Ring](kernel-ring.md)**