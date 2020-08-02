# Run an Clade Project Interactively

In most cases you will want to run MiGA by submitting batch jobs. But to gain some experience and see how thing work, you can run the following example interactively.  

Log into the HPCC with your user name and password.  

Create a directory for some example data and put some genomes in it. For example, assuming you start from your home directory, the following commands will place several *Dehalococoides* genomes in the directory `~/dehalo_genomes`:  

```
mkdir dehalo_genomes
cd mega_genomes

wget https://github.com/jfq3/data_sets/raw/master/Dehalococcoides_genomes/dehalococcoides_genomes.tar.gz
tar xzf dehalococcoides_genomes.tar.gz
rm dehalococcoides_genomes.tar.gz
```
Create a directory for your project in your home directory:  

```
cd
mkdir dehalo
```
Still from your home directory, start MiGA, create a new project named `dehalo`, and move into the dehalo directory:  

```
singularity shell MiGA
miga new -P ~/dehalo -t clade
cd dehalo
```
The `-t` argument is needed to specify what kind of genomes you are working with.  Acceptable types are :  

- `genomes` for the genomes from isolates
- `clade` for genomes of closely related species

For this example, we are using genomes from a single species, so we choose `clade`.  

Add a database as the reference for your project. The database on HPCC at MSU is:  

`/mnt/research/rdp/public/database/TypeMat/TypeMat_Lite`

so the command to add the reference database is:  

```
miga edit -P . -m "/mnt/research/rdp/public/database/TypeMat/TypeMat_Lite"
```

Add your data set to the MiGA project. In doing this, turn off mytaxa scan, as in the command below. Here the -i flag specifies that the data being uploaded are assembled genomes.  

```
miga add -P . -t genome -i assembly ~/dehalo_genomes/*.fna -m run_mytaxa_scan=false
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
