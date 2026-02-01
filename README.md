# 🚕 Autonomous Taxi Management Platform (UNIX)

![C](https://img.shields.io/badge/C-A8B9CC?style=for-the-badge&logo=c&logoColor=white)
![UNIX](https://img.shields.io/badge/UNIX-000000?style=for-the-badge&logo=linux&logoColor=white)
![Threads](https://img.shields.io/badge/Multithreading-Pthreads-blue?style=for-the-badge)

This project is a simulation platform for managing a fleet of autonomous taxis in a UNIX environment, developed for the **Operating Systems** course. The system focuses on efficient process management, multithreading, and Inter-Process Communication (IPC).

---

## System Architecture

The solution utilizes a distributed multi-process architecture based on three main components:


* **Controller (Central Manager):** The multithreaded core of the system. It manages simulation time, customer requests, and fleet monitoring through 4 distinct threads (`tClients`, `tControl`, `tTime`, `tFleet`).
* **Client (User Interface):** Allows users to schedule trips and check status updates. It uses I/O multiplexing (`select()`) to maintain a responsive interface while receiving background notifications.
* **Vehicle (Simulation):** Independent processes spawned via `fork/exec` that simulate physical taxi behavior and report real-time telemetry.

---

## 📡 Communication & Synchronization (IPC)

The system leverages native UNIX mechanisms to ensure data integrity and efficiency:

* **Named Pipes (FIFOs):** A public channel for requests to the Controller and private channels for direct notifications to Clients.
* **Anonymous Pipes:** Used for sending telemetry from Vehicles to the Controller (via `STDOUT` redirection).
* **Signals:**
    * `SIGINT`: Graceful shutdown of all system components.
    * `SIGALRM`: Management of simulated time progression.
    * `SIGUSR1` / `sigqueue`: Service cancellations and system closure notifications with data payload passing.
* **Synchronization:** Implementation of multiple **Mutexes** to protect concurrent access to user data, services, and fleet arrays, maximizing parallelism.

---

## Repository Structure

| File / Folder | Description |
| :--- | :--- |
| `src/controlador.c` | Central logic, thread management, and synchronization. |
| `src/cliente.c` | User interface with command and notification multiplexing. |
| `src/veiculo.c` | Trip simulation logic and telemetry reporting. |
| `include/comum.h` | Definitions of shared data structures and constants. |
| `Makefile` | Automated build script with `all` and `clean` rules. |

---

## Getting Started

### 1. Compilation
Compile all binaries (controller, client, and vehicle) simultaneously:
```bash
make all
```

### 2. Execution
Start the Controller first:
```bash
./controlador
```
In a new terminal, launch a Client:

```bash
./cliente [username]
```

---

**Developed by:** Miguel Zara and Guilherme Eça
