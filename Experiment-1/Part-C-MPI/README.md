# Part C — MPI Distributed Matrix Multiplication

## Objective

Implement the 4000 × 4000 matrix multiplication using MPI distributed-memory parallelism across four Ubuntu virtual machines.

The reference setup uses:
- 1 Master VM
- 3 Worker VMs
- 4 MPI processes
- 1000 matrix rows assigned to each process

## MPI Cluster

| Host | Hostname | Reference IP | MPI Role |
|---|---|---|---|
| Master | master | 192.168.125.128 | Rank 0 |
| Worker 1 | worker1 | 192.168.125.129 | Rank 1 |
| Worker 2 | worker2 | 192.168.125.130 | Rank 2 |
| Worker 3 | worker3 | 192.168.125.131 | Rank 3 |

The IP addresses may differ on another VM setup. Use the actual addresses assigned to the VMs.

## 1. VM Setup

Create four Ubuntu VMs and connect them to the same virtual network.

Set unique hostnames:

Master:
```bash
sudo hostnamectl set-hostname master
```

Worker 1:
```bash
sudo hostnamectl set-hostname worker1
```

Worker 2:
```bash
sudo hostnamectl set-hostname worker2
```

Worker 3:
```bash
sudo hostnamectl set-hostname worker3
```

Check the address on each VM:

```bash
hostname -I
```

## 2. Network Connectivity

From the Master VM:

```bash
ping -c 4 192.168.125.129
ping -c 4 192.168.125.130
ping -c 4 192.168.125.131
```

The reference configuration expects successful connectivity to the three Worker nodes.

## 3. Install OpenSSH

Run on every VM:

```bash
sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable --now ssh
```

## 4. Install Open MPI

Run on every VM:

```bash
sudo apt update
sudo apt install openmpi-bin libopenmpi-dev -y
```

Verify:

```bash
mpicc --version
mpirun --version
```

## 5. Configure Passwordless SSH

On the Master:

```bash
ssh-keygen -t rsa
ssh-copy-id worker1
ssh-copy-id worker2
ssh-copy-id worker3
```

Test:

```bash
ssh worker1 hostname
ssh worker2 hostname
ssh worker3 hostname
```

Expected output identifies worker1, worker2 and worker3.

## 6. Working Directory

On the Master:

```bash
mkdir -p ~/parallel_lab/mpi
cd ~/parallel_lab/mpi
```

## 7. Hostfile

The `hosts` file contains:

```text
master slots=1
worker1 slots=1
worker2 slots=1
worker3 slots=1
```

## 8. Source Code

The complete implementation is in `matrix_mpi.c`.

The program uses:
- `MPI_Init` and `MPI_Finalize`
- `MPI_Comm_rank` and `MPI_Comm_size`
- `MPI_Scatter` to distribute rows of A
- `MPI_Bcast` to distribute B
- Local matrix multiplication
- `MPI_Gather` to collect C on Rank 0
- `MPI_Barrier` and `MPI_Wtime` for synchronization and timing

With four processes, each process receives 1000 rows.

## 9. Compile

On the Master:

```bash
mpicc -O2 matrix_mpi.c -o matrix_mpi
```

## 10. Copy the Executable

```bash
scp matrix_mpi worker1:~/matrix_mpi
scp matrix_mpi worker2:~/matrix_mpi
scp matrix_mpi worker3:~/matrix_mpi
```

## 11. Run

```bash
mpirun -np 4 --hostfile hosts sh -c '$HOME/matrix_mpi'
```

The output should show ranks computing 1000 rows and the final verification value 4000.00.

If Open MPI refuses root execution, the manual recommends running as a normal user rather than disabling Open MPI safety checks unless required by the controlled lab environment.

## Expected Output

```text
MPI Matrix Multiplication Completed
Matrix Size = 4000 x 4000
Number of MPI Processes = 4
Execution Time = <measured time> seconds
Verification C[0][0] = 4000.00
```

## Reference Result

```text
MPI Matrix Multiplication Completed
Matrix Size = 4000 x 4000
Number of MPI Processes = 4
Execution Time = 92.979510 seconds
Verification C[0][0] = 4000.00
```

Reference speedup over sequential:

```text
244.12 / 92.979510 = 2.63×
```

## MPI Data Flow

```text
Matrix A (4000 rows)
   |
   +---- MPI_Scatter ----+
   |                     |
Rank 0 → 1000 rows    Rank 1 → 1000 rows
Rank 2 → 1000 rows    Rank 3 → 1000 rows

Matrix B ---- MPI_Bcast ----> all ranks

Each rank computes local_C

   +---- MPI_Gather ----+
                        |
                 Complete C
                  on Rank 0
```

## Screenshots

Add the experiment screenshots to the `screenshots` folder.

Suggested names:

```text
screenshots/
├── 01-vm-network.png
├── 02-ssh-test.png
├── 03-mpi-version.png
├── 04-hostfile.png
├── 05-source-code.png
├── 06-compilation.png
└── 07-result.png
```
