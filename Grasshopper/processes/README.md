# Linux Processes

This section covers Linux processes, their lifecycle, signals, scheduling priority, terminal control, job management, and the `/proc` virtual filesystem.

## Lessons Completed

1. **ps (Processes)**

   * Take process snapshots with `ps`.
   * Monitor changing process activity with `top`.

2. **Controlling Terminal**

   * Understand how controlling terminals connect sessions with interactive input, signals, and shell job control.

3. **Process Details**

   * Understand the difference between a running process and a program stored on disk.
   * Examine process state and resource information.

4. **Process Creation**

   * Understand how `fork`, `exec`, PIDs, and parent relationships participate in Linux process creation.

5. **Process Termination**

   * Understand exit status, waiting, zombies, and reparenting during the process lifecycle.

6. **Signals**

   * Understand how Linux generates, blocks, delivers, and handles signals.
   * Use signals to control processes and notify processes about events.

7. **kill (Terminate Processes)**

   * Identify a process.
   * Send an appropriate signal with `kill`.
   * Use a safe escalation sequence when terminating processes.

8. **Niceness**

   * Understand how `nice` values affect CPU scheduling weight for ordinary Linux processes.

9. **Process States**

   * Interpret common Linux process state codes in `ps` output.

10. **`/proc` Filesystem**

    * Use the virtual `/proc` filesystem to inspect current process and kernel information.

11. **Job Control**

    * Manage foreground, background, and stopped jobs from an interactive shell.

---

## Skills Practiced

* Inspecting processes with `ps`
* Monitoring processes with `top`
* Understanding PIDs and parent-child relationships
* Understanding the process lifecycle
* Working with Linux signals
* Safely terminating processes
* Understanding process states
* Working with process priority and niceness
* Inspecting processes through `/proc`
* Managing foreground and background shell jobs
* Understanding the role of the controlling terminal

---

## Key Concepts

### Process vs Program

A **program** is executable code stored on disk.

A **process** is a running instance of a program with its own execution context and resources.

A single program can therefore have multiple running processes.

---

## Process Identification

Every process has a process ID (**PID**).

Processes can also have a parent process identified by a **PPID**.

The parent-child relationship is an important part of the Linux process hierarchy.

Useful commands include:

```bash
ps
ps aux
```

---

## Process Creation

Linux process creation involves concepts such as:

```text
fork()
  ↓
new process
  ↓
exec()
  ↓
new program image
```

`fork()` creates a new process based on an existing process, while `exec()` replaces a process's program image with another program.

---

## Process Lifecycle

A simplified process lifecycle is:

```text
created
   ↓
running / ready
   ↓
waiting or running
   ↓
terminated
```

After termination, the parent may collect the child's exit status.

If the terminated child has not yet been collected, it can temporarily remain as a **zombie**.

---

## Signals

Signals are asynchronous notifications used for process control and event handling.

Common signals include:

| Signal    | Typical purpose                            |
| --------- | ------------------------------------------ |
| `SIGTERM` | Request graceful termination               |
| `SIGKILL` | Force termination                          |
| `SIGSTOP` | Stop a process                             |
| `SIGCONT` | Continue a stopped process                 |
| `SIGHUP`  | Hangup / session-related notification      |
| `SIGINT`  | Interrupt from terminal, commonly `Ctrl-C` |

Signals can be sent using:

```bash
kill PID
```

---

## Process States

Linux processes can have different states.

Common `ps` state codes include:

| Code | Meaning               |
| ---- | --------------------- |
| `R`  | Running or runnable   |
| `S`  | Interruptible sleep   |
| `D`  | Uninterruptible sleep |
| `T`  | Stopped               |
| `Z`  | Zombie                |

A process may move between states during its lifetime.

---

## Niceness

The `nice` value influences CPU scheduling weight for ordinary processes.

A process can be started with a specific nice value:

```bash
nice -n 10 command
```

An existing process can have its nice value adjusted with:

```bash
renice 10 -p PID
```

The exact scheduling behavior is determined by the Linux scheduler.

---

## `/proc`

`/proc` is a virtual filesystem provided by the Linux kernel.

It exposes dynamic information about:

* running processes;
* process status;
* kernel information;
* system information.

Process-specific directories are commonly named by PID:

```text
/proc/<PID>/
```

Examples:

```bash
cat /proc/1/status
ls /proc/1/
```

---

## Controlling Terminal

An interactive shell can have a controlling terminal associated with its session.

This relationship matters for:

* interactive input;
* terminal-generated signals;
* foreground processes;
* background processes;
* shell job control.

---

## Job Control

Interactive shells can manage jobs in different states:

```text
foreground
background
stopped
```

Common job-control commands include:

```bash
jobs
fg
bg
```

Keyboard shortcuts such as `Ctrl-C` and `Ctrl-Z` can also interact with foreground jobs.

---

## What I Learned

After completing this section, I can:

* inspect processes with `ps`;
* monitor process activity with `top`;
* explain PIDs and parent-child relationships;
* distinguish programs from running processes;
* describe the basic Linux process lifecycle;
* explain `fork()` and `exec()`;
* understand process exit and zombie processes;
* explain the purpose of Linux signals;
* terminate processes using an appropriate signal;
* interpret common process state codes;
* understand the effect of niceness on ordinary process scheduling;
* inspect process information through `/proc`;
* understand controlling terminals and shell job control.

---

## Documentation

* [Commands Cheat Sheet](commands.md)
* [Practice Journal](practice.md)

