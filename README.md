# aws-parallel-cluster-playground

learn AWS ParallelCluster.  formerly CfnCluster (“cloud formation cluster”)

> framework that deploys and maintains high performance computing clusters on Amazon Web Services (AWS). Developed by AWS, CfnCluster facilitates both quick start proof of concepts (POCs) and production deployments. CfnCluster supports many different types of clustered applications and can easily be extended to support different frameworks.

## Notes

*  distributed as a Python package and is installed using the Python pip package manager.
* supports [Slurm](https://docs.aws.amazon.com/parallelcluster/latest/ug/slurm-workload-manager-v3.html) and [AWS Batch](https://docs.aws.amazon.com/parallelcluster/latest/ug/awsbatchcli-v3.html) schedulers

## Demo

install, configure cluster, create cluster, submit job to cluster

```sh
python3 -m pip install "aws-parallelcluster" --upgrade --user

# verify install
pcluster version

# configure cluster. prompts for scheduler type, region, etc.
# when done `cluster-config.yaml` is created
# <https://github.com/aws/aws-parallelcluster/tree/release-3.0/cli/tests/pcluster/example_configs> for example
# cluster configuration files
# <https://docs.aws.amazon.com/parallelcluster/latest/ug/cluster-configuration-file-v3.html> - configuration files spec
pcluster configure --config cluster-config.yaml

# create / provision the cluster
pcluster create-cluster --cluster-name test-cluster --cluster-configuration cluster-config.yaml

# login to cluster head node
pcluster ssh --cluster-name test-cluster -i /path/to/keyfile.pem

# run the command sinfo to verify that your compute nodes are set up and configured.
sinfo

# create job to run (hello-job.sh)
cat << EOF > hello-job.sh
#!/bin/bash
sleep 30
echo "Hello World from $(hostname)"
EOF

# submit job
sbatch hello-job.sh

# view job in job queue
squeue

# once no job in queue a `.out` file will be created with results (STDOUT)
cat slurm-2.out
```

## Resources

- [AWS ParallelCluster](https://docs.aws.amazon.com/parallelcluster/latest/ug/what-is-aws-parallelcluster.html)
- [aws/aws-parallelcluster](https://github.com/aws/aws-parallelcluster)
- [AWS Services used in CfnCluster](https://cfncluster.readthedocs.io/en/latest/aws_services.html#aws-services)
- [Running your first job on AWS ParallelCluster](https://docs.aws.amazon.com/parallelcluster/latest/ug/tutorials-running-your-first-job-on-version-3.html)
- [Cluster configuration file](https://docs.aws.amazon.com/parallelcluster/latest/ug/cluster-configuration-file-v3.html)
- [aws-parallelcluster/cli/tests/pcluster/example_configs/](https://github.com/aws/aws-parallelcluster/tree/release-3.0/cli/tests/pcluster/example_configs) - example cluster configuration files