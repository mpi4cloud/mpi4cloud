# mpi4cloud

_mpi4cloud_ is a service for running MPI-workloads on Azure, AWS, and GCP
using low-cost unreliable VMs ([SPOT](https://aws.amazon.com/ec2/spot)/[preemptible](https://cloud.google.com/compute/docs/instances/preemptible)).
_mpi4cloud_ makes it simple to monitor the resource usage
and cost of your jobs.

## Getting Started

First, [create an account here](https://app.mpi4cloud.com/signup).

### Install

Then, install the CLI tool

- On **Linux / macOS**:
```bash
bash <(curl -s releases.mpi4cloud.com/install.sh)
```

- On **Windows**:

```bash
todo
```


Then, log in to your _mpi4cloud_ account with the CLI:

```bash
mpi4cloud login
# Username: ...
# Password: ...
```

### Launch an AWS cluster

First choose an instance to run.
You can use [this nice tool](https://instances.vantage.sh/) to
compare the different offerings.

For this tutorial, we will use 4 `t2.micro` instances.

```bash
mpi4cloud create \
    --provider aws \
    --instance-type t2.micro \
    --nodes 4
```

> Note: The default instance name is `mpi4cloud-cluster`,
> but you can change it with the `--cluster-name` flag.
> Similarly, the default AWS AMI used is the latest Ubuntu 20.04 image,
> but you can use a custom AMI with `--image-id`. More on this in
> the [commands page](commands.md).


### Launch an Azure cluster

todo

### Launch a GCP cluster

todo
