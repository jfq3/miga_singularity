# Running MiGa Interactively

In most cases you will want to run MiGa by submitting batch jobs. But to gain some experience and see how thing work, you can run the following example interactively.  

Log into the HPCC with your user name and password.  

Create a directory for some example data and put some genomes in it. For example, assuming you start from your home directory, the folloiwng commands will place several *Psuedomonas* genomes in the directory `~/miga_genomes`:  

```
mkdir miga_genomes
cd mega_genomes
wget 
tar
```
Create a directory for your project and move into that directory, for example:  
```
mkdir pseudo
cd pseudo # Maybe not?
```
Start MiGa and create a new project named `pseudo`:  

```
singularity shell MiGa
miga new -P pseudo -t genomes
```
The `-t` argument is needed to specify what kind of genomes you are working with.  Acceptable types are :  

- `popgenome` for Metagenome-Assembled Genomes
- `cgenome` for Single-Cell Amplified Genomes
- `genome` for the genomes from isolates

For this example, we are using genomes from isolates.  

Add a database as the reference for your project. The database on HPCC at MSU is:  

`/mnt/research/rdp/public/database/TypeMat/TypeMat_Lite`

so the command is:  

```
miga edit -P . -m "/mnt/research/rdp/public/database/TypeMat/TypeMat_Lite"
```
Add your data set to the MiGa project. To do this with the default parameters, enter:  

```
miga add -P . -t genome -i assembly ~/miga_genome/*.fasta
```
Or you could overide some of the default parameters, e.g. turning off mytaxa scan and distance thus:  

```
miga add -P . -t genome -i assembly ~/test_miga/*.fasta \
    -m run_mytaxa_scan=false, run_distances=false
```

Afterall, in this case we did not include mytaxa in the insstallation.  

Launch daemon to start MiGA processing your data:  

```
miga daemon start -P .
```
After the job starts, you can monitor its progress by entering:  

```
ls
```
