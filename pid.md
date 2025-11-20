# PID

is a **Process ID (PID)** is a **unique, positive integer** assigned by the kernel to every running process in the operating system (OS).

- **Process** is an instance of a program that is being executed. When you run an application, the OS creates a new process to run it.
- **PID** serves as a **handle** or **address** for the kernel to reference, monitor, and control a specific running program.

## Key Uses

1. **System Calls**

    A program uses a PID to communicate with another process (e.g, using `kill()` to terminate or send a signal to a specific process).

2. **Monitoring / Troubleshooting**

    To identify which program is consuming resources (like in Task Manager or ps command).

3. **Parent-Child Relationship**

    When a process (parent) creates another process (child), the parent needs the child's PID to manage it.

## Special PID

- **PID 0 (Swapper / Scheduler)**

    Non-existent or kernel thread in modern linux kernel, referring to the process that manages memory swapping.

- **PID 1 (init/ systemd)**

    The first user-space process started by the kernel. It is the parent of all other processes and is responsible for managing the rest of the system's startup and shutdown. This is the closest thing to the kernel having a process, but it still a separate user-space program.

## Related

- **[Kernel](kernel.md)**