# Part A — Sequential Matrix Multiplication

## Objective

Implement a 4000 × 4000 matrix multiplication using a sequential C program and record its execution time as the baseline for comparison.

## Prerequisites

- Windows PowerShell
- WSL2 with Ubuntu
- Internet access for package installation
- GCC / build-essential
- Permission to run sudo commands

## Setup

### Verify WSL

Run in Windows PowerShell:

```powershell
wsl --status
wsl -l -v
wsl
```

Ubuntu should be available under WSL2.

### Install GCC

Inside Ubuntu:

```bash
sudo apt update
sudo apt install build-essential -y
gcc --version
```

### Create the Working Directory

```bash
mkdir -p ~/parallel_lab/sequential
cd ~/parallel_lab/sequential
```

## Source Code

The complete implementation is in `matrix_sequential.c`.

The program:
1. Allocates memory for A, B and C.
2. Initializes A and B to 1.0 and C to 0.0.
3. Performs triple-nested-loop matrix multiplication.
4. Measures execution time using `clock()`.
5. Verifies `C[0][0]`.
6. Frees the allocated memory.

## Compilation

```bash
gcc -O2 matrix_sequential.c -o matrix_sequential
```

Verify the executable:

```bash
ls -l
```

## Execution

```bash
./matrix_sequential
```

Expected output format:

```text
Initializing 4000 x 4000 matrices...

Sequential Matrix Multiplication Completed
Matrix Size = 4000 x 4000
Execution Time = <measured time> seconds
Verification C[0][0] = 4000.00
```

## Reference Result

The reference experiment records:

```text
Initializing 4000 x 4000 matrices...
Sequential Matrix Multiplication Completed
Matrix Size = 4000 x 4000
Execution Time = 244.120000 seconds
Verification C[0][0] = 4000.00
```

The recorded 244.120000 seconds is the sequential baseline.

## Algorithm

For every output element C[i][j]:

```text
C[i][j] = sum of A[i][k] × B[k][j], for k = 0 ... N-1
```

The three loops iterate over the output row, output column and dot-product index.

## Screenshots

Add the experiment screenshots to the `screenshots` folder.

Suggested names:

```text
screenshots/
├── 01-wsl-verification.png
├── 02-gcc-version.png
├── 03-source-code.png
├── 04-compilation.png
└── 05-result.png
```
