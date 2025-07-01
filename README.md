# HPC using AWS
This project looks into options in AWS to use High Performance Computers (HPCs), and implement these towards a toy problem to start with.

## Options in AWS for HPC
- EC2 Instances with HPC capabilities: Instances like the C5n, Hpc6id, or P4 are designed for HPC workloads with fast networking (like Elastic Fabric Adapter - EFA).
- AWS ParallelCluster: An AWS-supported open-source cluster management tool to easily deploy and manage HPC clusters on AWS.
- AWS Batch (less direct for HPC but useful for batch jobs).   

## Setting up AWS
- Signing up to create an AWS account.
- Configuring AWS CLI on local machine.
- Creating IAM user and root user credentials.
- Configuring AWS profile on AWS CLI.

## Installing and configuring AWS ParallelCluster for the toy problem
1. Installing ParallelCluster
'''
pip install aws-parallelcluster
'''
2. Configuring ParallelCluster
'''
pcluster configure
'''
3. Creating an HPC cluster
'''
pcluster create mycluster
'''
4. Connect the cluster to master node IP
'''
pcluster ssh mycluster -i path/to/key.pem
'''
5. Setting up environment on the cluster (I am using C++)
'''
module load gcc
module load openmpi
'''
6. Copying code files to master node
'''
scp -i path/to/key.pem your_code.cpp ec2-user@master_node_ip:~
'''
7. Compiling code on the cluster (example: advection_sim.cpp)
'''
mpic++ -o advection_sim advection_sim.cpp
'''
8. Running simulation using slurm scheduler
Example job.sh:
'''
#!/bin/bash
#SBATCH --job-name=advection
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=4
#SBATCH --time=00:10:00

srun ./advection_sim
'''
Submitting job by:
'''
sbatch job.sh
'''
9. Monitoring job
'''
squeue
'''
