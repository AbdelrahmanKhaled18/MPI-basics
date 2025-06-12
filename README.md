
# 🧵 MPI (Message Passing Interface) – C++ Guide

## 📌 Overview

**MPI (Message Passing Interface)** is a standardized and portable message-passing system designed to allow processes to communicate with each other in a parallel computing environment. It is widely used in **High-Performance Computing (HPC)** for developing scalable and efficient parallel applications on distributed-memory systems.

* **Language Bindings:** C, C++, Fortran (C++ support via C interface)
* **Implementations:** MPICH, OpenMPI, Intel MPI, MVAPICH, etc.
* **Model:** Based on the **SPMD (Single Program Multiple Data)** model

---

## 🧠 Why Use MPI?

* Leverages **multiple processors/nodes** for performance
* Ideal for **scientific computing, simulations**, and **data-intensive tasks**
* Runs on clusters, supercomputers, and multi-core systems

---

## ⚙️ Installation

### Ubuntu/Debian:

```bash
sudo apt update
sudo apt install mpich
```

### macOS (with Homebrew):

```bash
brew install mpich
```

### Compile and Run:

```bash
mpic++ program.cpp -o program
mpirun -np 4 ./program
```

---

## 📘 MPI Basics

### Hello World Example (C++)

```cpp
#include <mpi.h>
#include <iostream>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int world_rank, world_size;
    MPI_Comm_rank(MPI_COMM_WORLD, &world_rank);
    MPI_Comm_size(MPI_COMM_WORLD, &world_size);

    std::cout << "Hello from process " << world_rank << " out of " << world_size << std::endl;

    MPI_Finalize();
    return 0;
}
```

---

## 🔑 Most Commonly Used MPI APIs (in C++ via C interface)

### 1. **Initialization and Finalization**

| Function       | Description                               |
| -------------- | ----------------------------------------- |
| `MPI_Init`     | Initializes the MPI execution environment |
| `MPI_Finalize` | Terminates the MPI environment            |

### 2. **Process Identification**

| Function        | Description                        |
| --------------- | ---------------------------------- |
| `MPI_Comm_rank` | Gets the rank (ID) of the process  |
| `MPI_Comm_size` | Gets the total number of processes |

### 3. **Point-to-Point Communication**

| Function    | Description                        |
| ----------- | ---------------------------------- |
| `MPI_Send`  | Blocking send                      |
| `MPI_Recv`  | Blocking receive                   |
| `MPI_Isend` | Non-blocking send                  |
| `MPI_Irecv` | Non-blocking receive               |
| `MPI_Wait`  | Waits for a non-blocking operation |

### 4. **Collective Communication**

| Function        | Description                              |
| --------------- | ---------------------------------------- |
| `MPI_Bcast`     | Broadcast from one to all processes      |
| `MPI_Scatter`   | Distributes unique data from root to all |
| `MPI_Gather`    | Gathers data from all to root            |
| `MPI_Allgather` | Gathers data from all to all             |
| `MPI_Reduce`    | Reduces values (e.g., sum, max) to root  |
| `MPI_Allreduce` | Reduces and shares result with all       |

### 5. **Synchronization**

| Function      | Description                         |
| ------------- | ----------------------------------- |
| `MPI_Barrier` | Blocks until all processes reach it |

### 6. **Derived Data Types**

| Function                 | Description                    |
| ------------------------ | ------------------------------ |
| `MPI_Type_create_struct` | Create a custom data type      |
| `MPI_Type_commit`        | Finalize the derived data type |

---

## 📌 Advanced Concepts

### 🔄 Non-Blocking Communication

Improves performance by overlapping computation and communication:

```cpp
MPI_Request request;
MPI_Isend(data, count, MPI_INT, dest, tag, MPI_COMM_WORLD, &request);
// Do computation
MPI_Wait(&request, MPI_STATUS_IGNORE);
```

### 📤 Collective Communication Example

```cpp
int data;
MPI_Bcast(&data, 1, MPI_INT, 0, MPI_COMM_WORLD); // Root sends `data` to all
```

### 🧮 Reduction Example

```cpp
int local_sum = 10, global_sum;
MPI_Reduce(&local_sum, &global_sum, 1, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);
```

---

## 🧪 Debugging and Profiling

* Use tools like **`mpiP`**, **`TAU`**, or **Intel VTune**
* Compile with debug flags: `mpic++ -g`
* Check return codes from MPI calls

---

## 📚 Resources

* [MPI Standard (official)](https://www.mpi-forum.org/)
* [MPICH Documentation](https://www.mpich.org/documentation/)
* [OpenMPI Documentation](https://www.open-mpi.org/doc/)
* **Book:** *Using MPI* by Gropp, Lusk, and Skjellum
* **Tutorial:** [https://mpitutorial.com/](https://mpitutorial.com/)

---

## 🏁 Best Practices

* Minimize communication overhead
* Use non-blocking I/O where appropriate
* Combine messages to reduce traffic
* Profile performance to optimize
* Always `MPI_Finalize()` to clean up

---

## 🧩 Real-World Applications of MPI

* Climate Modeling
* Astrophysics Simulations
* Computational Biology
* Machine Learning with HPC
* Large-scale Financial Risk Analysis

---

## 📦 Example Project Structure

```
mpi-app/
├── src/
│   └── main.cpp
├── Makefile
└── README.md
```

**Makefile Example:**

```makefile
all:
	mpic++ src/main.cpp -o mpi_app

run:
	mpirun -np 4 ./mpi_app
```

---

## ✨ Summary

MPI is **the foundation of distributed computing** in scientific applications. With its **flexible, efficient**, and **standardized API**, MPI allows C++ developers to harness the power of modern supercomputing architectures with precision and scalability.

---

Would you like a version of this as a GitHub README with Markdown formatting and links, or a sample mini project structure as well?
