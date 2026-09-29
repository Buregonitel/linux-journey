# Linux Processes - Command Cheat Sheet

A practical reference for inspecting, monitoring, controlling, and understanding Linux processes and shell jobs.

---

## 1. `ps`

`ps` displays a snapshot of currently running processes.

### Current shell processes

```bash
ps
```

### Detailed process listing

```bash
ps aux
```

### Process hierarchy

```bash
ps -ef --forest
```

Useful fields commonly include:

| Field     | Meaning           |
| --------- | ----------------- |
| `PID`     | Process ID        |
| `PPID`    | Parent Process ID |
| `USER`    | Process owner     |
| `STAT`    | Process state     |
| `%CPU`    | CPU usage         |
| `%MEM`    | Memory usage      |
| `COMMAND` | Command/program   |

---

## 2. `top`

`top` provides a continuously updating view of system processes.

```bash
top
```

It can be used to monitor:

* CPU usage;
* memory usage;
* process IDs;
* process states;
* running processes.

Unlike `ps`, which normally provides a snapshot, `top` continuously refreshes its display.

---

## 3. Process IDs

Every process has a PID.

Find the PID of a command:

```bash
ps
```

Or search a detailed process listing:

```bash
ps aux | grep process-name
```

Inspect a specific process:

```bash
ps -p PID
```

Example:

```bash
ps -p 1234
```

---

## 4. Parent Processes

Processes can have parent-child relationships.

Inspect PID and PPID:

```bash
ps -o pid,ppid,stat,cmd
```

Example:

```text
PID    PPID  STAT  CMD
1234   1000  S     bash
```

Here:

```text
PID  → process ID
PPID → parent process ID
```

---

## 5. Process Creation: `fork()` and `exec()`

Linux process creation commonly involves two concepts:

```text
fork()
  ↓
child process
  ↓
exec()
  ↓
new program image
```

### `fork()`

Creates a new process based on the calling process.

### `exec()`

Replaces the current process's program image with another program.

These are system-call concepts rather than ordinary shell commands.

---

## 6. Process Exit Status

A process can terminate with an exit status.

In Bash, the status of the most recently executed command is available through:

```bash
echo $?
```

Example:

```bash
true
echo $?
```

A successful command normally returns:

```text
0
```

A non-zero status generally indicates some form of failure or other condition.

---

## 7. `wait`

A parent process can wait for a child process and collect its termination status.

In Bash, `wait` can wait for a background process:

```bash
command &
wait
```

Wait for a specific PID:

```bash
command &
PID=$!

wait "$PID"
```

This is important for understanding how terminated child processes are collected.

---

## 8. Signals

Signals are notifications delivered to processes.

Common signals:

| Signal    | Number* | Typical purpose     |
| --------- | ------: | ------------------- |
| `SIGHUP`  |       1 | Hangup              |
| `SIGINT`  |       2 | Interrupt           |
| `SIGTERM` |      15 | Request termination |
| `SIGKILL` |       9 | Force termination   |
| `SIGSTOP` |     19† | Stop                |
| `SIGCONT` |     18† | Continue            |

* Signal numbers can vary between architectures.

† Common Linux values; scripts should generally prefer signal names rather than relying on numbers.

---

## 9. `kill`

Despite its name, `kill` does not necessarily terminate a process immediately. It sends a signal to a process.

### Send the default termination signal

```bash
kill PID
```

Equivalent to:

```bash
kill -TERM PID
```

### Send a signal by name

```bash
kill -TERM PID
```

### Force termination

```bash
kill -KILL PID
```

Use `SIGKILL` only when a process does not respond appropriately to a graceful termination request.

### List available signals

```bash
kill -l
```

---

## 10. Safe Process Termination

A practical escalation sequence is:

```text
identify process
      ↓
send SIGTERM
      ↓
wait / verify
      ↓
send SIGKILL only if necessary
```

Example:

```bash
kill -TERM PID
ps -p PID
```

If the process remains and forceful termination is justified:

```bash
kill -KILL PID
```

---

## 11. `nice`

Start a command with a specified nice value:

```bash
nice -n 10 command
```

Check the resulting process:

```bash
ps -o pid,ni,cmd
```

The `NI` field shows the nice value.

Nice values influence the scheduling weight of ordinary processes.

---

## 12. `renice`

Change the nice value of an existing process:

```bash
renice 10 -p PID
```

Inspect it:

```bash
ps -o pid,ni,cmd -p PID
```

Changing nice values may require additional privileges, especially when increasing a process's scheduling priority.

---

## 13. Process States

Use `ps` to inspect process state:

```bash
ps -o pid,stat,cmd
```

Common state codes:

| Code | Meaning               |
| ---- | --------------------- |
| `R`  | Running or runnable   |
| `S`  | Interruptible sleep   |
| `D`  | Uninterruptible sleep |
| `T`  | Stopped               |
| `Z`  | Zombie                |

A `STAT` value can contain additional characters that provide more information about the process.

---

## 14. `/proc`

The `/proc` filesystem provides dynamic information from the kernel.

List process entries:

```bash
ls /proc
```

PID directories are numeric:

```text
/proc/1
/proc/1000
/proc/1234
```

Inspect process status:

```bash
cat /proc/PID/status
```

Example:

```bash
cat /proc/1/status
```

---

## 15. Useful `/proc` Files

### `/proc/PID/status`

Human-readable process status information:

```bash
cat /proc/PID/status
```

### `/proc/PID/cmdline`

Command-line arguments:

```bash
cat /proc/PID/cmdline
```

### `/proc/PID/fd/`

File descriptors associated with the process:

```bash
ls -l /proc/PID/fd
```

### `/proc/PID/exe`

Symbolic link to the executable:

```bash
ls -l /proc/PID/exe
```

The exact information available depends on the process and system configuration.

---

## 16. Job Control

Start a command in the background:

```bash
command &
```

List shell jobs:

```bash
jobs
```

Bring a job to the foreground:

```bash
fg
```

Bring a specific job to the foreground:

```bash
fg %1
```

Continue a stopped job in the background:

```bash
bg %1
```

---

## 17. Keyboard Job Control

### `Ctrl-C`

Typically sends `SIGINT` to the foreground process group.

### `Ctrl-Z`

Typically suspends the foreground job by sending `SIGTSTP`.

After suspending a job:

```bash
bg
```

continues it in the background.

Or:

```bash
fg
```

brings it back to the foreground.

---

## 18. Controlling Terminal

A controlling terminal connects an interactive session with processes and the shell's job-control mechanisms.

Terminal-generated signals are normally delivered to the foreground process group.

This explains why commands such as:

```text
Ctrl-C
Ctrl-Z
```

can affect the currently running foreground job.

---

## Quick Reference

| Command            | Purpose                       |
| ------------------ | ----------------------------- |
| `ps`               | Process snapshot              |
| `ps aux`           | Detailed process listing      |
| `top`              | Live process monitoring       |
| `ps -p PID`        | Inspect a specific process    |
| `ps -o ...`        | Select process fields         |
| `kill PID`         | Send a signal                 |
| `kill -TERM PID`   | Request termination           |
| `kill -KILL PID`   | Force termination             |
| `kill -l`          | List signals                  |
| `nice`             | Start with a nice value       |
| `renice`           | Change nice value             |
| `jobs`             | List shell jobs               |
| `fg`               | Bring a job to foreground     |
| `bg`               | Continue a job in background  |
| `wait`             | Wait for a background process |
| `echo $?`          | Display last exit status      |
| `/proc/PID/status` | Inspect process status        |

---

## Useful Commands and Pipelines

### Find a process

```bash
ps aux | grep "process-name"
```

### Show PID, parent PID, state, and command

```bash
ps -o pid,ppid,stat,cmd
```

### Monitor a specific process

```bash
ps -p PID -o pid,ppid,stat,ni,cmd
```

### Inspect process status through `/proc`

```bash
cat /proc/PID/status
```

### Start and inspect a background job

```bash
command &
jobs
```

### Start a process with lower scheduling priority

```bash
nice -n 10 command
```

### Gracefully terminate a process

```bash
kill -TERM PID
```

Then verify:

```bash
ps -p PID
```

