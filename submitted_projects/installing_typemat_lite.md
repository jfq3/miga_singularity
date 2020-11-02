This script is provided as a convinient way of obtaining the latest version of the TypeMat_Lite indexed reference database. By attaching this database to a genome project, reference genomes submitted to the project are classified relative to the closest match in the database.

The database is installed in the directory `~/.miga_db`. The resource requst section is for a SLURM job scheduler.

```
#!/bin/bash --login
########## SBATCH Lines for Resource Request ##########
#SBATCH --time=02:00:00         # limit of wall clock time - how long the job will run (same as -t)
#SBATCH --nodes=1               # number of different nodes - could be an exact number or a range of nodes (same as -N)
#SBATCH --ntasks=1              # number of tasks - how many tasks (nodes) that you require (same as -n)
#SBATCH --cpus-per-task=2       # number of CPUs (or cores) per task (same as -c)
#SBATCH --mem-per-cpu=16G       # memory required per allocated CPU (or core) - amount of memory (in bytes)
#SBATCH --job-name get_db       # you can give your job a name for easier identification (same as -J)
#SBATCH -A colej

########## Command Lines to Run ##########
## Download the latest version of TypeMat_Lite to ~/.miga_db.

cd
singularity shell ~/MiGA << EOF
miga download
exit
EOF
```
