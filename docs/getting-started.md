# Getting Started with mpi4cloud

First, [create an account here](https://app.mpi4cloud.com/signup).

## Install

Then, install the CLI tool

- On **Linux / macOS**:
```bash
bash <(curl -s releases.mpi4cloud.com/install.sh)
```

- On **Windows**:

```bash
todo
```


Then, log in to your mpi4cloud account with the CLI:

```bash
mpi4cloud login
# Username: ...
# Password: ...
```


## Launch an AWS cluster

First, you need to install and configure the AWS CLI so that
mpi4cloud can launch, provision, and monitor instances for you.
You should follow the [installation instructions](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
and then the [configuration instructions](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html).

Then, choose an instance type.
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
> but you can use a custom AMI with `--image-id`.
> You can run `mpi4cloud run --help` for more details on default options.

## Launch an Azure cluster

todo

## Launch a GCP cluster

todo


## Run your first job

mpi4cloud attempts to create a seamless development
experience by zipping your local directory (ignoring
whatever Git ignores),
uploading it to all VMs in the cluster,
and then running `mpirun` to launch the job.

For example, to launch a job using all nodes in the cluster
we just created (from any of the cloud providers), you'd run:

```bash
mpi4cloud run -np 4 python3 main.py
```

All arguments following `run` are passed to `mpirun`.
However, you will sometimes need to provide arguments to
mpi4cloud (i.e., to change the cluster name).
To do this, bash provides a "[double dash](https://unix.stackexchange.com/questions/11376/what-does-double-dash-mean)"
for separating these options.

For example, to specify the name of the cluster to run the job on, you'd run:

```bash
mpi4cloud run --cluster-name my-cluster -- -np 4 python3 main.py
```

All flags before the `--` are passed to mpi4cloud and the ones after
are passed to `mpirun`.

> Note: the `-np` flag tells `mpirun` how many processes to launch.
> You do not need to provide a hostfile; it is generated for you.
> Check out the [mpirun documentation](https://www.open-mpi.org/doc/v4.1/man1/mpirun.1.php)
> for more control over how processes are mapped to nodes (aka "hosts").


## Provisioning a cluster

Oftentimes, you will need more dependencies than are installed on
the VM.
To do this, create a `setup.sh` file in the root directory
of your project and fill it with any installation commands.
For example:

```bash
# setup.sh

sudo apt update
sudo apt upgrade -y
```

> Note: an MPI implementation and the mpi4cloud Python
> library along with some other useful packages
> for working with MPI in Python will automatically be installed for you.


## Listing nodes in a cluster

You can list the nodes in a cluster with the following command:

```bash
mpi4cloud list-nodes --cluster-name ...
# ID                   TYPE       STATE      PUBLIC IP       PRIVATE IP
# i-abcdefgh           t2.micro   running    10.0.0.45       10.0.3.10
# ...
```

You can omit the `--cluster-name` flag to list the nodes for the default cluster.


## SSH into a node

First, use the `list-nodes` command to get the IDs of all nodes.
Then, you can SSH into a particular one with its ID:

```bash
mpi4cloud ssh <instance-id>
```

## Working with Docker

In general, most projects can get away with running directly on the host OS,
installing the necessary dependencies with `setup.sh`.

However, you may need more fine-grain control over the dependencies
in your application, especially if you are working with multiple collaborators
or switching between on-prem and cloud environments.

mpi4cloud provides the `run-docker` command to launch a job using Docker
containers.

For example, to run a job using the latest PyTorch container,
you would run:

```bash
mpi4cloud run-docker --nodes 4 -- pytorch/pytorch
```

To use your own [Dockerfile](https://docs.docker.com/engine/reference/builder/),
you would replace the image name with `.`.
However, mpi4cloud will automatically look for a Dockerfile in the
root directory of your project, so you can just run:

```bash
mpi4cloud run-docker --nodes 4
```

## Next Steps

To see examples of how to use mpi4cloud
with a variety of tools leveraging model/data
parallelism, check out the [GitHub repo](https://github.com/mpi4cloud/mpi4cloud/tree/main/examples).

If you have issues running something or seek general advice,
feel free to [leave an issue](https://github.com/mpi4cloud/mpi4cloud/issues).
