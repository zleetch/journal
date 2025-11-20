# Kernel Ring

![kernel ring](https://www.mindevice.net/uploads/kernel_rings_8d7b755657.webp)

Kernel ring model is critical for **system stability and security**:

1. **Isolation and security** 
    
    It strictly isolates low-trust user program the highly privileged kernel. This prevents a buggy or malicious application from corrupting the kernel, crashing the OS, or accessing the private memory or data of other programs.

2. **Fault tolerance**

    Limiting the access of user applications, the system can tolerate errors in user space code (Ring 3) without suffering a total system failure.

3. **Controlled access**

    Ensure all access to critical hardware and system resources is mediated and approved by the kernel, giving the OS complete control over the machine.

## Ring 0

- **Privilege Level Highest** (Most Trusted).
- This is where the **Operating System Kernel** resides.
- **Full access** to all system resources, including CPU, physical memory, and all hardware devices (via device drivers).
- If any error or malicious code running on this ring can **crash the entire system** or compromise its security completely.

## Ring 1 & Ring 2

- Act as intermediate rings were originally intended for things like **device drivers** or other system level services that required more privilege than user applications but less than the full kernel.
- **Rarely used** in general purpose operating system today.

## Ring 3

- **Privilege level lowest** (Least Trusted).
- This is where **user applications** resides.
- **Cannot directly access** hardware or critical kernel memory.
- When user application needs to perform a privleged operation (reading a file from disk, accessing network), it must make a **System Call** controlled request that switches the CPU into Rint 0, allowing the kernel to perform the operation on its behalf and return the result.
- If the application crash its isolated and typically only affects that single application, not the entire OS.