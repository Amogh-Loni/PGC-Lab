# PGC Lab

This repository contains the laboratory work for Parallel and Grid Computing (PGC).

## Experiment 1 — Matrix Multiplication

Experiment 1 implements the same 4000 × 4000 matrix multiplication using three computing models included in this repository:

- Part A — Sequential Matrix Multiplication
- Part B — OpenMP Matrix Multiplication
- Part C — MPI Distributed Matrix Multiplication

Part D (CUDA) from the laboratory manual is not included in this repository.

## Repository Structure

```text
PGC-Lab/
└── Experiment-1/
    ├── Part-A-Sequential/
    │   ├── README.md
    │   ├── matrix_sequential.c
    │   └── screenshots/
    │       └── .gitkeep
    ├── Part-B-OpenMP/
    │   ├── README.md
    │   ├── matrix_openmp.c
    │   └── screenshots/
    │       └── .gitkeep
    └── Part-C-MPI/
        ├── README.md
        ├── matrix_mpi.c
        ├── hosts
        └── screenshots/
            └── .gitkeep
```

## Problem Definition

Matrices A and B are both 4000 × 4000 matrices whose elements are initialized to 1.0.

```text
C = A × B
C[0][0] = 1×1 + 1×1 + ... + 1×1 (4000 terms)
C[0][0] = 4000.00
```

## Reference Results

| Implementation | Model | Reference Time | Verification |
|---|---|---:|---:|
| Sequential | Single CPU execution | 244.120000 s | 4000.00 |
| OpenMP | Shared-memory parallelism | 30.830434 s | 4000.00 |
| MPI | Distributed-memory parallelism | 92.979510 s | 4000.00 |

## Parts

### Part A — Sequential

[Open Part A](./Experiment-1/Part-A-Sequential/)

### Part B — OpenMP

[Open Part B](./Experiment-1/Part-B-OpenMP/)

### Part C — MPI

[Open Part C](./Experiment-1/Part-C-MPI/)

## Screenshots

Each part contains its own `screenshots` directory. Actual experiment screenshots can be added there after running the programs.

## Source

The implementation, procedure, and reference results are based on the provided Experiment 1 laboratory manual. The manual defines the sequential baseline, OpenMP shared-memory implementation, and MPI distributed-memory implementation used here.
