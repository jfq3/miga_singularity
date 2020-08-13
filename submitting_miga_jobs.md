# Submitting MiGA Jobs

Below is an example submision script for running the same job as in the interactive genomes project. It assumes that the genomes to be processed are in the same directory, i.e. `~/miga_genomes`. The project directory is `~/miga_batch`. After starting the MiGA singularity container, the miga commands are the same as in the interactive example except this time we add a reference database (TypeMat_Lite) from RDP's research directory. The location and perhaps name of such a reference database would be different on other clusters.

Actual wall time for this example was ~ 22 minutes.

```text
#!/bin/bash --login
########## SBATCH Lines for Resource Request ##########
#SBATCH --time=01:00:00         # limit of wall clock time - how long the job will run (same as -t)
#SBATCH --nodes=1               # number of different nodes - could be an exact number or a range of nodes (same as -N)
#SBATCH --ntasks=1              # number of tasks - how many tasks (nodes) that you require (same as -n)
#SBATCH --cpus-per-task=10      # number of CPUs (or cores) per task (same as -c)
#SBATCH --mem-per-cpu=2G        # memory required per allocated CPU (or core) - amount of memory (in bytes)
#SBATCH --job-name TestingMiGA  # you can give your job a name for easier identification (same as -J)

########## Command Lines to Run ##########

mkdir ~/miga_batch
cd ~/miga_batch

singularity shell ~/MiGA << EOF
miga new -P ~/miga_batch -t genomes
miga edit -P . -m "/mnt/research/rdp/public/database/TypeMat/TypeMat_Lite"
miga add -P . -t genome -i assembly ~/miga_genomes/*.fasta -m run_mytaxa_scan=false,run_distances=true
time miga daemon start -t -P . --shutdown-when-done
exit
EOF
```

