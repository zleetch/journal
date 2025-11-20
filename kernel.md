# Kernel

Kernel is neither a process, a daemon, nor an application. It is a privileged, memory resident environment that forms the foundation of the OS. Unlike user programs, it is not scheduled, doesn't have a PID, and is not started or stopped like conventional task. Instead, it is always present loaded into memory at boot and governs all interactions between hardware and software.

## Purpose

Ensure that every process runs reliably, securely, and efficiently. If the kernel fails to respond to a system call, allocate memory, access storage, or enforce isolation, it has failed in its core purpose.

## Related

- **[System Call](system-call.md)**
- **[PID](pid.md)**