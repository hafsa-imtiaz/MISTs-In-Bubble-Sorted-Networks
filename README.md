# Constructing MISTs in Bubble-Sorted Networks

## 📘 Project Overview  
This project implements both **serial** and **parallel** algorithms for constructing **Multiple Independent Spanning Trees (MISTs)** in **Bubble-Sorted Networks (Bn)**. It is based on the research article _“Constructing Multiple Independent Spanning Trees in Bubble-Sorted Networks”_ authored by:

**Shih-Shun Kao**, **Ralf Klasing**, **Ling-Ju Hung**, **Chia-Wei Lee**, and **Sun-Yuan Hsieh**.

The parallel implementation uses **MPI** and **OpenMP** to improve performance over the serial version, which is written purely in C++. This project demonstrates how parallel computing can accelerate graph problems with factorial growth.

---

## 👩‍💻 Authors
- Hafsa Imtiaz (22i-0959)  
- Areen Zainab (22i-1115)  
- Areeba Riaz (22i-1244)

---

## 🧠 Problem Description  
A **Bubble-Sorted Network (Bn)** is a structured graph topology used in parallel architectures. For a given Bn, the task is to construct **n − 1 independent spanning trees (MISTs)** to increase fault tolerance. The factorial complexity of the problem makes it well-suited for distributed and parallel solutions.

---

## 🔧 Methodology

### 🔹 Serial Implementation  
- Written in C++
- Generates all permutations of `{1, 2, ..., n}` to represent vertices
- Constructs each tree sequentially using a `Parent1()` function and BFS
- Writes `tree_t.txt` file for each tree with parent/level info

### 🔹 Parallel Implementation  
- **MPI**: Each tree `t` is assigned to an MPI process in round-robin fashion  
- **OpenMP**: Each MPI process uses multithreading to process permutations  
- Reduces total computation time by distributing permutation work
- Each process writes its own `tree_t.txt` output file

### 🔹 Cluster Setup  
- Implemented using **VMware Ubuntu nodes** on a local machine  
- Simulates a distributed cluster with 2GB RAM per node  
- MPI handles inter-process, OpenMP handles intra-process parallelism

---

## 📊 Speed-Up Analysis

| N  | Sequential | Seq + OpenMP | MPI | MPI + OpenMP |
|----|------------|---------------|-----|----------------|
| 3  | 1x         | ~3x           | ~6x | ~8x            |
| 4  | 1x         | ~9x           | ~7x | ~9x            |
| 5  | 1x         | ~2x           | ~8x | ~2x            |
| 6  | 1x         | ~2.2x         | ~5x | ~2x            |
| 7  | 1x         | ~2x           | ~3x | ~4x            |
| 8  | 1x         | ~1.5x         | ~4x | ~8x            |
| 9  | 1x         | ~1.5x         | ~4x | ~10x           |
| 10 | 1x         | ~2x           | ~5x | ~12x           |
| 11 | 1x         | ~1.3x         | ~6x | ~14x           |

---


## 📈 Discussion  
The **serial implementation** was straightforward but slow for large `N`, given the factorial number of permutations. The **parallel implementation** significantly improved performance by:
- Distributing tree workloads using **MPI**
- Speeding up permutation computations with **OpenMP**

Some performance bottlenecks remained due to:
- Memory overhead of storing permutations in all processes
- Virtualized cluster setup limiting scalability

---

## ⚠️ Warning: Observations on Tree Validity for n ≥ 5

While testing our implementation, we observed that for network sizes **n ≥ 5**, some of the generated spanning trees may not strictly satisfy all the expected properties of valid **Multiple Independent Spanning Trees (MISTs)**.

### 🧭 Key Observations:
- A few trees appeared to contain **cycles**, which would violate the tree structure.
- In some cases, we noticed **disconnected components**, where certain permutations were not properly connected to the root.
- Multiple parent relationships were occasionally seen, which may indicate a logic error or ambiguity in the original formulation for higher `n`.

These findings are based on our practical implementation and review of tree outputs. While the core algorithm follows the method described in the referenced article, the correctness of the trees for **larger network sizes** may warrant further verification and refinement.

> ⚠️ These are our own experimental observations and should not be considered definitive. We recommend users inspect the outputs critically when working with **n ≥ 5**.
 
---

## ✅ How to Run

### 🔹 Serial Version
```bash
g++ serial.cpp -o serial
./serial
```

### 🔹 Parallel with MPI
```bash
mpic++ parallel_mpi.cpp -o mpi_version
mpirun -np 8 ./mpi_version
```

### 🔹 Parallel with MPI + OpenMP
```bash
mpic++ -fopenmp parallel_mpi_openmp.cpp -o hybrid
export OMP_NUM_THREADS=4
mpirun -np 8 ./hybrid
```
---


## ✅ Conclusion  
This project demonstrates how parallel computing principles can be applied to a computationally intensive graph problem. The combination of **MPI and OpenMP** results in significant speedups, validating the approach even on limited hardware. Future work can target better scaling, memory efficiency, and improved hardware utilization.

---

### 📄 License
This project is developed for academic purposes only.
It is based on the article: “Constructing Multiple Independent Spanning Trees in Bubble-Sorted Networks”.

All code and documentation are intended strictly for educational use and may not be used for commercial purposes without prior written permission.