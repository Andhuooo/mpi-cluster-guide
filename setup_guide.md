# MPI Cluster Setup Guide

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Network Configuration](#network-configuration)
3. [User Setup](#user-setup)
4. [OpenMPI Installation](#openmpi-installation)
5. [SSH Configuration](#ssh-configuration)
6. [Firewall Settings](#firewall-settings)
7. [Testing the Cluster](#testing-the-cluster)
8. [Troubleshooting](#troubleshooting)

## Prerequisites <a name="prerequisites"></a>
- Two or more Linux machines (CentOS/RHEL 7+/Ubuntu 18.04+)
- Stable network connection between nodes
- Sudo/root access on all machines
- Basic terminal proficiency

## Network Configuration <a name="network-configuration"></a>
### 1. Set Static IPs
On each node:
```bash
sudo nmtui  # Or edit /etc/sysconfig/network-scripts/ifcfg-ens33

2. Configure /etc/hosts
bash

sudo nano /etc/hosts

Add (example for 2 nodes):
text

192.168.1.10 node1
192.168.1.11 node2

User Setup <a name="user-setup"></a>

Create a dedicated MPI user:
bash

sudo useradd -m mpi_user
sudo passwd mpi_user

OpenMPI Installation <a name="openmpi-installation"></a>
CentOS/RHEL:
bash

sudo yum install -y epel-release
sudo yum install -y openmpi openmpi-devel

Ubuntu/Debian:
bash

sudo apt update
sudo apt install -y openmpi-bin libopenmpi-dev

Verify Installation:
bash

mpirun --version
which mpicc

SSH Configuration <a name="ssh-configuration"></a>
1. Generate SSH Keys
bash

su - mpi_user
ssh-keygen -t rsa -b 4096

2. Copy Keys Between Nodes
bash

ssh-copy-id mpi_user@node1
ssh-copy-id mpi_user@node2

3. Test Passwordless Login
bash

ssh node2 hostname

Firewall Settings <a name="firewall-settings"></a>
CentOS/RHEL:
bash

sudo firewall-cmd --permanent --add-port=1024-65535/tcp
sudo firewall-cmd --reload

Ubuntu:
bash

sudo ufw allow 1024:65535/tcp

Testing the Cluster <a name="testing-the-cluster"></a>
1. Create Hostfile
bash

cat > hostfile <<EOF
node1 slots=2
node2 slots=2
EOF

2. Run Test Program
bash

mpicc mpi_hello.c -o mpi_hello
mpirun -np 4 -hostfile hostfile ./mpi_hello

Troubleshooting <a name="troubleshooting"></a>
Common Errors:

    "ORTE lost connection"
    bash

mpirun -mca btl tcp,self -mca routed direct -np 2 ./program

"libmpi.so not found"
bash

export LD_LIBRARY_PATH=/usr/lib64/openmpi/lib:$LD_LIBRARY_PATH

SSH Connection Issues
bash

    chmod 700 ~/.ssh
    chmod 600 ~/.ssh/authorized_keys

Debug Commands:
bash

mpirun -display-map -np 2 hostname
orte-ps --help
