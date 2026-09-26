# Part B — OpenMP Matrix Multiplication

## Objective

Parallelize the 4000 × 4000 matrix multiplication using OpenMP shared-memory parallelism.

## Prerequisites

- Working WSL2 Ubuntu environment from Part A
- GCC
- Multiple logical CPUs
- Sequential baseline already recorded

## Setup

Enter WSL:

```powershell
wsl
```

Check available logical CPUs:

```bash
nproc
```

For the reference experiment, 8 logical CPUs are available.

Set and verify the OpenMP thread count:

```bash
export OMP_NUM_THREADS=8
echo $OMP_NUM_THREADS
```

Expected output:

```text
8
```

Create the working directory:

```bash
mkdir -p ~/parallel_lab/openmp
cd ~/parallel_lab/openmp
```

## Source Code

The complete implementation is in `matrix_openmp.c`.

The key OpenMP directive is:

```c
#pragma omp parallel for private(j, k)
```

This divides the outer `i` loop among OpenMP threads while the matrices remain in shared memory.

## Compilation

```bash
gcc -O2 -fopenmp matrix_openmp.c -o matrix_openmp
```

The `-fopenmp` option enables OpenMP support.

## Execution

```bash
./matrix_openmp
```

To monitor CPU utilization during execution:

```bash
htop
```

## Expected Output

```text
OpenMP Matrix Multiplication Completed
Matrix Size = 4000 x 4000
Number of Threads Used = 8
Execution Time = <measured time> seconds
Verification C[0][0] = 4000.00
```

## Reference Result

```text
OpenMP Matrix Multiplication Completed
Matrix Size = 4000 x 4000
Number of Threads Used = 8
Execution Time = 30.830434 seconds
Verification C[0][0] = 4000.00
```

Reference speedup over sequential:

```text
244.12 / 30.830434 = 7.92×
```

## How It Works

The matrix multiplication algorithm remains the same as Part A. The outer loop is distributed among OpenMP threads. Each thread works on different output rows while A, B and C are in shared memory.

## Screenshots

Add the experiment screenshots to the `screenshots` folder.

Suggested names:

```text
screenshots/
├── 01-nproc.png
├── 02-openmp-threads.png
├── 03-source-code.png
├── 04-compilation.png
├── 05-htop.png
└── 06-result.png
```
