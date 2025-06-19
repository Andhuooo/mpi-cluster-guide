# MPI Cluster Setup Guide

A complete tutorial for setting up a 2-node MPI cluster on Linux systems.

## Features
- Step-by-step OpenMPI installation
- Passwordless SSH configuration
- Firewall rules for MPI communication
- Sample MPI programs with explanations

## Quick Start
```bash
# Compile and run the example MPI program
mpicc examples/mpi_hello.c -o mpi_hello
mpirun -np 2 ./mpi_hello
