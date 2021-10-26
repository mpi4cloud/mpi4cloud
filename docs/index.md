# mpi4cloud

_mpi4cloud_ is a service for running MPI-workloads on Azure, AWS, and GCP
using low-cost unreliable VMs ([SPOT](https://aws.amazon.com/ec2/spot)/[preemptible](https://cloud.google.com/compute/docs/instances/preemptible)).
_mpi4cloud_ makes it simple to monitor the resource usage
and cost of your jobs.

_mpi4cloud_ does not provide a fully managed service,
so everything happens on your cloud provider account.

The components that _mpi4cloud_ provides are:

- a CLI for interacting with clusters
- a web application for tracking/predicting your spending, monitoring resource usage / logs, and controlling clusters
- a Python library with utilities for managing unreliable workloads