# miga_singularity

These instructions are for installing MiGA in a Singularity container from a Docker image. This overcomes several disadvantages over using a Docker container in a cluster environment, the more important of which are: 

- Docker containers grant superuser privileges, so it is hard to limit access to a user. Singularity runs as the user without granting superuser access.
- The Docker API client acts through the docker daemon so that resource requests made by a submission script (e.g. Slurm) and actual resource used do not match. Singularity containers instead run as child processes, i.e. without a daemon, so Slurm resource requests are honored. 

While these instructions are written for installing MiGA in a user's home directory on Michigan State University's HPCC, they should work on any cluster including Singularity.

Also included are tutorials for how to run several project types, and how to submit jobs to the cluster.