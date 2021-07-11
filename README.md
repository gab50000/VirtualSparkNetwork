# About

This repository provides a Vagrantfile for setting up a small virtual Spark cluster consisting of a master node and two worker nodes.

# Setup

First, run

```
./download_spark.sh
```

Afterwards, set up the virtual machines via

```
vagrant up
```

In order to increase the number of workers, change the variable `NUM_WORKERS` in the Vagrantfile.
