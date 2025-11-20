# Interrupt Request (IRQ)

Is a **hardware generated signal** sent to the CPU to temporarily halt its current task and request immediate attention for an urgent event. Kernel is critically involved because it containes the **software routines** (Interrupt Handlers) necessary to process, prioritize, and respond to every hardware interrupt.

Feature,IRQ (Interrupt Request),System Call
Initiator,Hardware (asynchronous event).,Software (user application).
Timing,Asynchronous (can happen at any moment).,Synchronous (occurs at a specific point in a program's execution).
Goal,To handle an external device event.,To request a privileged service from the kernel.

## Relation with kernel

Kernel is the owner and IRQ is the manager.

1. **IRQ number mapping**

    Every hardware interrupt source is associate with a unique **IRQ Number** (e.g, IRQ 0 for system, IRQ 1 for keyboard). The kernel maintains the table, often called **Interrupt Descriptor Table (IDT)**, which maps these IRQ numbers to the memory address of the specific kernel code responsible for handling that interrupt.

2. **Device driver registration**

    Part of the kernel (kernel module), must **register** an **Interrupt Service Routine (ISR)**, or **Interrupt Handler**, with the kernel for the specific IRQ number its device uses. This is typically done when the device is initialized.

3. **Handling Process**

    When an IRQ occurs, the kernel handles it in multiple phases to ensure minimal disruption:

    - **Top Half (Critical Phase):** The CPU jumps to the kernel **Interrupt Handler**. This code runs immediately with interrupts often disabled (or masked) to prevent being interrupted by lower priority IRQs. The top half performs onlu the most **time critical task**, such as acknowledging the interrupt to the hardware controller and reading the necessary data registers from the devices. This code must be executed as quickly as possible.

    - **Bottom Half (Deferred Phase):** After the top half quickly finishes, it schedules the rest of the interrupt processing work (the less time-critical tasks) to be done later. This work is done by the bottom half (often implemented via **softirqs, tasklets, or workqueues**). The bottom half runs with interrupts enabled and often in a process context, allowing it to perform complex operations like data manipulation or waking up waiting processes without blocking the system for too long.