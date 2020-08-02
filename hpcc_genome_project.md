# Running MiGA Interactively

In most cases you will want to run MiGA by submitting batch jobs. But to gain some experience and see how thing work, you can run the following example interactively.  

Log into the HPCC with your user name and password.  

Create a directory for some example data and put some genomes in it. For example, assuming you start from your home directory, the following commands will place several *Psuedomonas* genomes in the directory `~/miga_genomes`:  

```
mkdir miga_genomes
cd mega_genomes
wget https://github.com/jfq3/data_sets/raw/master/Pseudomonas_genomes/pseudomonads.tar.gz
tar xzf pseudomonads.tar.gz
rm pseudomonads.tar.gz
```
Create a directory for your project in your home directory:  

```
cd
mkdir pseudo
```
Still from your home directory, start MiGA, create a new project named `pseudo`, and move into the pseudo directory:  

```
singularity shell MiGA
miga new -P ~/pseudo -t genomes
cd pseudo
```
The `-t` argument is needed to specify what kind of genomes you are working with.  Acceptable types are :  

- `popgenome` for Metagenome-Assembled Genomes
- `cgenome` for Single-Cell Amplified Genomes
- `genomes` for the genomes from isolates

**Problem: It seems only genomes is actually accepted.**

For this example, we are using genomes from isolates.  

Add a database as the reference for your project. The database on HPCC at MSU is:  

`/mnt/research/rdp/public/database/TypeMat/TypeMat_Lite`

so the command to add the reference database is:  

```
miga edit -P . -m "/mnt/research/rdp/public/database/TypeMat/TypeMat_Lite"
```

Add your data set to the MiGA project. In doing this, turn off mytaxa scan and make sure distances is turned on, as in the command below. Here the -i flag specifies that the data being uploaded are assembled genomes.  **Note:** I once got an error with the first command below.

```
miga add -P . -t genome -i assembly ~/miga_genomes/*.fasta -m run_mytaxa_scan=false, run_distances=true

miga add -P . -t genome -i assembly ~/miga_genomes/*.fasta -m run_mytaxa_scan=false


```

Launch the daemon to start MiGA processing your data:  

```
miga daemon start -P .
```

After the job starts, you can display the dataset information:  

```
miga about -P .
```
Also after the job starts, you can monitor its progress by entering:  

```
miga ls -P . -p 
```
