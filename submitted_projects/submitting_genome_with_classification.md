# Submit a Genome Project with Classification

After creating a new project, edit it to include a reference project database. Then add the reference genomes and start the daemon. 

```
#!/bin/bash --login
########## SBATCH Lines for Resource Request ##########
#SBATCH --time=04:00:00         # limit of wall clock time - how long the job will run (same as -t)
#SBATCH --nodes=1               # number of different nodes - could be an exact number or a range of nodes (same as -N)
#SBATCH --ntasks=1              # number of tasks - how many tasks (nodes) that you require (same as -n)
#SBATCH --cpus-per-task=10      # number of CPUs (or cores) per task (same as -c)
#SBATCH --mem-per-cpu=2G        # memory required per allocated CPU (or core) - amount of memory (in bytes)
#SBATCH --job-name class        # you can give your job a name for easier identification (same as -J)

########## Command Lines to Run ##########

mkdir ~/miga_class
cd ~/miga_class

singularity shell ~/MiGA << EOF
miga new -P ~/miga_class -t genomes
miga edit -P . -m ref_project=~/.miga_db/TypeMat_Lite
miga add -P . -t genome -i assembly ~/miga_genomes/misc/*.fasta -m run_mytaxa_scan=false,run_distances=true
time miga daemon start -t -P . --shutdown-when-done
exit
EOF

```
A summary classification can be obtained with the miga command:
```

Singularity> miga ls -P ~/miga_class/ -m tax
```
which returns:
```
Acidobacterium_capsulatum       d:Bacteria p:Acidobacteria c:Acidobacteriia o:Acidobacteriales
Acinetobacter_baumanii  d:Bacteria p:Proteobacteria c:Gammaproteobacteria
Bacillus_anthracis      d:Bacteria p:Firmicutes c:Bacilli o:Bacillales
Bacillus_cereus d:Bacteria p:Firmicutes c:Bacilli o:Bacillales
Bifidobacterium_bifidum d:Bacteria p:Actinobacteria c:Actinobacteria o:Bifidobacteriales f:Bifidobacteriaceae
Campylobacter_jejuni    d:Bacteria p:Proteobacteria c:Epsilonproteobacteria o:Campylobacterales
Cytophaga_hutchinsonii  d:Bacteria p:Bacteroidetes c:Cytophagia
Gemmatimonas_aurantiaca d:Bacteria p:Gemmatimonadetes c:Gemmatimonadetes o:Gemmatimonadales
Lacunisphaera_limnophila        d:Bacteria p:Verrucomicrobia c:Opitutae o:Opitutales
```

Individual `*intax.txt` files for each genome can be found in directory  `~/miga_class/data/09.distances/05.taxonomy/`.  For example:

```
less ~/miga_class/data/09.distances/05.taxonomy/Bifidobacterium_bifidum.intax.txt
```

returns:

```
Closest relative: Bifidobacterium_scardovii_JCM_12489___DSM_13734_GCA_000770985 with AAI: 68.94928718008717.

      Rank  Taxonomy                                         P-value               Signif.
      ----  --------                                         -------               -------
      root  ?                                                0.0                   ****
    domain  Bacteria                                         0.0                   ****
    phylum  Actinobacteria                                   0.000191394343233856  ****
     class  Actinobacteria                                   0.00118380945629829   ****
     order  Bifidobacteriales                                0.0024385057063869    ****
    family  Bifidobacteriaceae                               0.0481321329836251    ***
     genus  Bifidobacterium                                  0.256907918054866     *
   species  Bifidobacterium scardovii                        0.876302544835897
subspecies  ?                                                0.960990997377189
   dataset  Bifidobacterium scardovii JCM 12489 = DSM 13734  0.965570284256043

Significance at p-value below: *0.5, **0.1, ***0.05, ****0.01.
```


Times reported for this project were (real = wall time):  

 _   | Time
-----|---
real | 94m39.246s
user | 414m44.775s
sys  | 7m26.888s
