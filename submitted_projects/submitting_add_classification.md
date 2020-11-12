It is possible to classify reference genomes already in a project. You so this by editing the project to include a reference database.  

The following script will classify the genomes submitted in the previous project.  

```
#!/bin/bash --login
########## SBATCH Lines for Resource Request ##########
#SBATCH --time=01:00:00         # limit of wall clock time - how long the job will run (same as -t)
#SBATCH --nodes=1               # number of different nodes - could be an exact number or a range of nodes (same as -N)
#SBATCH --ntasks=1              # number of tasks - how many tasks (nodes) that you require (same as -n)
#SBATCH --cpus-per-task=12      # number of CPUs (or cores) per task (same as -c)
#SBATCH --mem-per-cpu=2G        # memory required per allocated CPU (or core) - amount of memory (in bytes)
#SBATCH --job-name classify      # you can give your job a name for easier identification (same as -J)

########## Command Lines to Run ##########

mkdir ~/miga_batch
cd ~/miga_batch

singularity shell ~/MiGA << EOF
miga edit -P . -m ref_project=$HOME/.miga_db/TypeMat_Lite
time miga daemon start -t -P . --shutdown-when-done
exit
EOF
```
Times reported for this project were (real = wall time):  
 _   | Time
-----|---
real | 45m40.816s
user | 226m55.092s
sys  | 4m12.932s

