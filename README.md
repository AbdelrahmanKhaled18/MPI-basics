
# 🧵 MPI (Message Passing Interface) – C++ Guide

## 📌 Overview

**MPI (Message Passing Interface)** is a standardized and portable communication protocol designed for **parallel and distributed computing**. It is primarily used in **High-Performance Computing (HPC)** to coordinate processes executing across multiple nodes.

* **Standardized:** By MPI Forum
* **Languages Supported:** C, C++ (via C bindings), Fortran
* **Popular Implementations:** OpenMPI, MPICH, Intel MPI
* **Communication Model:** Message-passing (processes use send/receive)

---

## ⚙️ Installation

### On Ubuntu/Debian:

```bash
sudo apt install mpich
```

### On macOS:

```bash
brew install mpich
```

---

## 🧪 Compile and Run

```bash
mpic++ program.cpp -o program
mpirun -np 4 ./program
```

---

## 🧠 Basic MPI Program

```cpp
#include <mpi.h>
#include <iostream>

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);

    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank); // Get process ID
    MPI_Comm_size(MPI_COMM_WORLD, &size); // Get number of processes

    std::cout << "Hello from process " << rank << " of " << size << std::endl;

    MPI_Finalize();
    return 0;
}
```

---

## 🧩 MPI API Reference – C++ (C Bindings)

### 1. **Initialization & Finalization**

#### `int MPI_Init(int *argc, char ***argv);`

* Initializes MPI environment.
* Must be called before any other MPI call.

#### `int MPI_Finalize();`

* Cleans up the MPI environment.
* Must be called at the end of all MPI programs.

---

### 2. **Process Info**

#### `int MPI_Comm_rank(MPI_Comm comm, int *rank);`

* `comm`: Communicator (e.g. `MPI_COMM_WORLD`)
* `rank`: Output, ID of the calling process (0 to size-1)

#### `int MPI_Comm_size(MPI_Comm comm, int *size);`

* `comm`: Communicator
* `size`: Output, total number of processes in the communicator

---

### 3. **Point-to-Point Communication**

#### `int MPI_Send(const void *buf, int count, MPI_Datatype datatype, int dest, int tag, MPI_Comm comm);`

* `buf`: Starting address of send buffer
* `count`: Number of elements to send
* `datatype`: Type of elements (e.g., `MPI_INT`)
* `dest`: Destination process rank
* `tag`: Message tag
* `comm`: Communicator

#### `int MPI_Recv(void *buf, int count, MPI_Datatype datatype, int source, int tag, MPI_Comm comm, MPI_Status *status);`

* `buf`: Receive buffer
* `count`: Max number of elements to receive
* `datatype`: Expected data type
* `source`: Rank of sender (`MPI_ANY_SOURCE` for wildcard)
* `tag`: Message tag (`MPI_ANY_TAG` for wildcard)
* `comm`: Communicator
* `status`: Output status object (can be `MPI_STATUS_IGNORE`)

#### `int MPI_Isend(const void *buf, int count, MPI_Datatype datatype, int dest, int tag, MPI_Comm comm, MPI_Request *request);`

* Same as `MPI_Send`, but non-blocking
* `request`: Used to track the progress

#### `int MPI_Irecv(void *buf, int count, MPI_Datatype datatype, int source, int tag, MPI_Comm comm, MPI_Request *request);`

* Non-blocking receive

#### `int MPI_Wait(MPI_Request *request, MPI_Status *status);`

* Blocks until the non-blocking request completes
* `request`: Pointer to an MPI request object
* `status`: Can be `MPI_STATUS_IGNORE`

---

### 4. **Collective Communication**

#### `int MPI_Bcast(void *buffer, int count, MPI_Datatype datatype, int root, MPI_Comm comm);`

* `buffer`: Buffer (send from root, receive at others)
* `count`: Number of elements
* `datatype`: Type of elements
* `root`: Rank of broadcasting process
* `comm`: Communicator

#### `int MPI_Gather(const void *sendbuf, int sendcount, MPI_Datatype sendtype, void *recvbuf, int recvcount, MPI_Datatype recvtype, int root, MPI_Comm comm);`

* Gathers data from all processes to root

#### `int MPI_Scatter(const void *sendbuf, int sendcount, MPI_Datatype sendtype, void *recvbuf, int recvcount, MPI_Datatype recvtype, int root, MPI_Comm comm);`

* Scatters data from root to all processes

#### `int MPI_Reduce(const void *sendbuf, void *recvbuf, int count, MPI_Datatype datatype, MPI_Op op, int root, MPI_Comm comm);`

* `sendbuf`: Local input
* `recvbuf`: Output at root
* `count`: Number of elements
* `datatype`: Type of elements
* `op`: Operation (e.g. `MPI_SUM`, `MPI_MAX`)
* `root`: Rank of root process

#### `int MPI_Allreduce(const void *sendbuf, void *recvbuf, int count, MPI_Datatype datatype, MPI_Op op, MPI_Comm comm);`

* Like `MPI_Reduce` but distributes result to all processes

#### `int MPI_Allgather(const void *sendbuf, int sendcount, MPI_Datatype sendtype, void *recvbuf, int recvcount, MPI_Datatype recvtype, MPI_Comm comm);`

* Gathers from all to all

---

### 5. **Synchronization**

#### `int MPI_Barrier(MPI_Comm comm);`

* All processes wait until every process reaches the barrier

---

### 6. **Status and Request Handling**

#### `MPI_Status`

Structure that stores info about a received message:

* `MPI_SOURCE`: Sender’s rank
* `MPI_TAG`: Message tag
* `MPI_ERROR`: Error code

#### `MPI_Request`

Used to track progress of non-blocking communication

---

### 7. **Derived Data Types (Optional but Advanced)**

#### `int MPI_Type_create_struct(int count, const int array_of_blocklengths[], const MPI_Aint array_of_displacements[], const MPI_Datatype array_of_types[], MPI_Datatype *newtype);`

* Creates custom data types

#### `int MPI_Type_commit(MPI_Datatype *datatype);`

* Finalizes the datatype before use in communication

---

## 📚 Useful Constants

| Constant             | Description                |
| -------------------- | -------------------------- |
| `MPI_COMM_WORLD`     | Default communicator       |
| `MPI_ANY_SOURCE`     | Wildcard for source rank   |
| `MPI_ANY_TAG`        | Wildcard for tag           |
| `MPI_STATUS_IGNORE`  | Ignore the returned status |
| `MPI_SUM`            | Reduction operation: sum   |
| `MPI_MAX`, `MPI_MIN` | Other reductions           |

---

## 📦 Sample Project Structure

```
mpi-cpp/
├── src/
│   └── main.cpp
├── Makefile
└── README.md
```

**Makefile:**

```makefile
all:
	mpic++ src/main.cpp -o mpi_app

run:
	mpirun -np 4 ./mpi_app
```

---

## 📘 Resources

* [MPI Tutorial](https://mpitutorial.com/)
* [MPI Standard Documentation](https://www.mpi-forum.org/)
* [OpenMPI](https://www.open-mpi.org/)
* Book: *Using MPI* by Gropp, Lusk, Skjellum

---

## ✅ Summary

MPI is the **core technology for distributed-memory parallel programming**, especially in scientific and HPC domains. Using MPI in C++ requires understanding its C bindings, mastering communication and synchronization primitives, and applying performance-aware design.


