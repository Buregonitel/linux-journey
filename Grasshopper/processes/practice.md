# Linux Processes - Practice Journal

Hands-on practice for inspecting, monitoring, creating, terminating, and managing Linux processes.

---

## 1. `ps` and `top`

### Goal

Learn how to inspect a snapshot of processes and monitor changing process activity.

### Practice

Display processes associated with the current shell:

```bash
ps
```

Display a broader process listing:

```bash
ps aux
```

Start the interactive process monitor:

```bash
top
```

Exit `top` with:

```text
q
```

### Result

Record:

* PID;
* process owner;
* CPU usage;
* memory usage;
* process state;
* command.

### What I learned

`ps` provides a process snapshot, while `top` continuously updates process information.

---

## 2. Controlling Terminal

### Goal

Understand how an interactive terminal connects a shell session with foreground processes and signals.

### Practice

Run a command that stays active:

```bash
sleep 100
```

While it is running, press:

```text
Ctrl-C
```

Observe what happens.

Run it again:

```bash
sleep 100
```

Press:

```text
Ctrl-Z
```

Then inspect the jobs:

```bash
jobs
```

### Result

Record:

* what happened after `Ctrl-C`;
* what happened after `Ctrl-Z`;
* the job state shown by `jobs`.

### What I learned

The controlling terminal participates in interactive input, signal generation, and shell job control.

---

## 3. Process Details

### Goal

Understand the difference between a program stored on disk and a running process.

### Practice

Start a long-running command:

```bash
sleep 1000 &
```

Inspect it:

```bash
ps -p $! -o pid,ppid,stat,ni,cmd
```

The shell variable `$!` contains the PID of the most recent background process.

Inspect the executable:

```bash
ls -l /proc/$!/exe
```

Inspect process information:

```bash
cat /proc/$!/status
```

### Result

Record:

```text
PID:
PPID:
state:
nice value:
command:
```

### What I learned

A running process has an execution context and resources that do not exist merely because executable code is stored on disk.

---

## 4. Process Creation

### Goal

Understand the role of `fork`, `exec`, PIDs, and parent-child relationships.

### Practice

Start a child process from the shell:

```bash
sleep 1000 &
```

Find its PID:

```bash
echo $!
```

Inspect the parent relationship:

```bash
ps -o pid,ppid,stat,cmd -p $!
```

Compare the PID and PPID.

### Result

Record:

```text
PID:
PPID:
parent process:
child process:
```

### What I learned

Linux processes form parent-child relationships. Process creation and execution are conceptually associated with `fork()` and `exec()`.

---

## 5. Process Termination

### Goal

Understand process exit status, waiting, and the concept of zombie processes.

### Practice

Run a successful command:

```bash
true
echo $?
```

Run a command that returns a non-zero status:

```bash
false
echo $?
```

Practice waiting for a background process:

```bash
sleep 2 &
PID=$!
wait "$PID"
echo $?
```

### Result

Record the exit statuses.

### What I learned

Processes terminate with an exit status, and a parent can wait for a child and collect its termination status.

A terminated child that has not yet been collected can temporarily exist as a zombie.

---

## 6. Signals

### Goal

Understand how signals can control processes and notify them about events.

### Practice

Start a background process:

```bash
sleep 1000 &
PID=$!
```

Send a termination request:

```bash
kill -TERM "$PID"
```

Verify:

```bash
ps -p "$PID"
```

List available signals:

```bash
kill -l
```

### Result

Record:

* the PID;
* signal sent;
* process state after the signal.

### What I learned

Signals provide a mechanism for asynchronous process control and event notification.

---

## 7. `kill`

### Goal

Practice identifying a process and terminating it using a safe escalation sequence.

### Practice

Start a test process:

```bash
sleep 1000 &
PID=$!
```

Confirm its identity:

```bash
ps -p "$PID" -o pid,ppid,stat,cmd
```

First request graceful termination:

```bash
kill -TERM "$PID"
```

Verify:

```bash
ps -p "$PID"
```

If the controlled test process does not terminate and forceful termination is appropriate, use:

```bash
kill -KILL "$PID"
```

### Result

Record whether `SIGTERM` was sufficient.

### What I learned

`kill` sends signals; it does not inherently mean "forcefully terminate." A graceful termination request should generally be attempted before `SIGKILL`.

---

## 8. Niceness

### Goal

Understand how nice values influence CPU scheduling weight.

### Practice

Start a command with a non-default nice value:

```bash
nice -n 10 sleep 1000 &
```

Inspect it:

```bash
ps -p $! -o pid,ni,cmd
```

For an existing process, practice changing its nice value where permitted:

```bash
renice 10 -p PID
```

Verify:

```bash
ps -p PID -o pid,ni,cmd
```

### Result

Record:

```text
initial nice value:
new nice value:
```

### What I learned

Nice values influence the scheduling weight of ordinary processes. They are one mechanism for adjusting relative CPU scheduling priority.

---

## 9. Process States

### Goal

Learn to interpret common process state codes shown by `ps`.

### Practice

Display process states:

```bash
ps -eo pid,stat,cmd
```

Look for common codes such as:

```text
R
S
D
T
Z
```

Create a stopped test job:

```bash
sleep 1000
```

Press:

```text
Ctrl-Z
```

Then inspect it:

```bash
jobs
ps -eo pid,stat,cmd
```

Continue it when finished:

```bash
fg
```

Then terminate it with:

```text
Ctrl-C
```

### Result

Record the state observed while the job was stopped.

### What I learned

The `STAT` field provides information about the current process state, and a process can transition between states during its lifecycle.

---

## 10. `/proc` Filesystem

### Goal

Explore the virtual `/proc` filesystem and inspect live process information.

### Practice

Inspect the `/proc` directory:

```bash
ls /proc
```

Inspect PID 1:

```bash
cat /proc/1/status
```

Start a test process:

```bash
sleep 1000 &
PID=$!
```

Inspect its status:

```bash
cat /proc/"$PID"/status
```

Inspect its executable:

```bash
ls -l /proc/"$PID"/exe
```

Inspect its file descriptors:

```bash
ls -l /proc/"$PID"/fd
```

### Result

Record which information you found in:

```text
/proc/PID/status
/proc/PID/exe
/proc/PID/fd/
```

### What I learned

`/proc` is a virtual filesystem that exposes current information supplied by the Linux kernel about processes and the system.

---

## 11. Job Control

### Goal

Practice foreground, background, and stopped shell jobs.

### Practice

Start a foreground job:

```bash
sleep 100
```

Suspend it:

```text
Ctrl-Z
```

List jobs:

```bash
jobs
```

Continue it in the background:

```bash
bg
```

List jobs again:

```bash
jobs
```

Bring it back to the foreground:

```bash
fg
```

Terminate it:

```text
Ctrl-C
```

### Result

Record the job state after:

1. `Ctrl-Z`;
2. `bg`;
3. `fg`;
4. `Ctrl-C`.

### What I learned

Interactive shells can manage jobs in foreground, background, and stopped states.

---

# Final Mini Exercise

## Goal

Combine process inspection, job control, signals, niceness, and `/proc`.

### Step 1 - Start a background process

```bash
sleep 1000 &
PID=$!
```

### Step 2 - Inspect the process

```bash
ps -p "$PID" -o pid,ppid,stat,ni,cmd
```

Record:

```text
PID:
PPID:
STAT:
NI:
COMMAND:
```

### Step 3 — Inspect `/proc`

```bash
cat /proc/"$PID"/status
```

Then:

```bash
ls -l /proc/"$PID"/exe
```

### Step 4 - Change the nice value

```bash
renice 10 -p "$PID"
```

Verify:

```bash
ps -p "$PID" -o pid,ni,cmd
```

### Step 5 — Send a graceful termination signal

```bash
kill -TERM "$PID"
```

Verify:

```bash
ps -p "$PID"
```

### Step 6 - Practice job control separately

Start another test command:

```bash
sleep 1000
```

Press:

```text
Ctrl-Z
```

Then:

```bash
jobs
bg
jobs
fg
```

Finish it with:

```text
Ctrl-C
```

---

# Reflection

After completing the exercises, answer:

1. What information does `ps` provide?
2. How is `top` different from `ps`?
3. What is the difference between a PID and a PPID?
4. What roles do `fork()` and `exec()` play in process creation?
5. What is an exit status?
6. What is a zombie process?
7. What is a signal?
8. Why should `SIGTERM` normally be tried before `SIGKILL`?
9. What does the nice value affect?
10. What do common `STAT` codes such as `R`, `S`, `T`, and `Z` mean?
11. What kind of information can be found under `/proc/PID/`?
12. How does the controlling terminal interact with job control?

---

## Key Takeaway

Linux process management combines several related mechanisms:

```text
Process
  │
  ├── PID / PPID
  ├── State
  ├── Resources
  ├── Signals
  ├── Scheduling / nice value
  ├── /proc information
  └── Terminal / job control
```

Understanding these relationships makes it easier to inspect running systems, control processes safely, and diagnose unexpected process behavior.

