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


Then, log in to your _mpi4cloud_ account with the CLI:

```bash
mpi4cloud login
# Username: ...
# Password: ...
```


## Launch an AWS cluster

First, you need to install and configure the AWS CLI so that
_mpi4cloud_ can launch, provision, and monitor instances for you.
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
> but you can use a custom AMI with `--image-id`. More on this in
> the [commands page](commands.md).

## Launch an Azure cluster

todo

## Launch a GCP cluster

todo


## Run your first job

_mpi4cloud_ attempts to create a seamless development
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
_mpi4cloud_ (i.e., to change the cluster name).
To do this, bash provides a "[double dash](https://unix.stackexchange.com/questions/11376/what-does-double-dash-mean)"
for separating these options.

For example, to specify the name of the cluster to run the job on, you'd run:

```bash
mpi4cloud run --cluster-name my-cluster -- -np 4 python3 main.py
```

All flags before the `--` are passed to _mpi4cloud_ and the ones after
are passed to `mpirun`.

> Tip: You can use the `--oversubscribe` `mpirun` flag to use more nodes
> than there are total processors.
> This comes in handy when testing out programs before scaling up.
> You can find full documentation of `mpirun` [here](https://www.open-mpi.org/doc/v4.1/man1/mpirun.1.php).


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

> Note: an MPI implementation and the _mpi4cloud_ Python
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

In general, VM images are relatively stable
and most projects can get away with running directly on the host OS.

However, you made need more fine-grain control over the dependencies
in your application, especially if you are working with multiple collaborators
or switching between on-prem and cloud environments.

todo