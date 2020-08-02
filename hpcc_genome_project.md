# Running a Genome Project Interactively

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
- `scgenome` for Single-Cell Amplified Genomes
- `genomes` for the genomes from isolates

**Problem: It seems only genomes is actually accepted.**

For this example, we are using genomes from isolates.  

Add a database as the reference for your project. The database on the HPCC at MSU is:  

`/mnt/research/rdp/public/database/TypeMat/TypeMat_Lite`

so the command to add the reference database is:  

```
miga edit -P . -m "/mnt/research/rdp/public/database/TypeMat/TypeMat_Lite"
```

Add your data set to the MiGA project. In doing this, turn off mytaxa scan and make sure distances is turned on, as in the command below. If desitred, calculating distances could be turned off by setting run_distances=false. There must be no space after the comma(s) in the items listed for the -m flag or an error will occur. Here the -i flag specifies that the datasets being uploaded are assembled genomes.

```
miga add -P . -t genome -i assembly ~/miga_genomes/*.fasta -m run_mytaxa_scan=false,run_distances=true
```

Launch the daemon to start MiGA processing your data:  

```
miga daemon start -P .
```

After the job starts, you can display the information about the job:  

```
miga about -P .
```
Also after the job starts, you can list the datasets being processed by entering:  

```
miga ls -P . -i 
```

And you can monitor the job progress by entering:  

```
miga ls -P . -p
```
This should give someting like:

```
name           raw_reads  trimmed_reads  read_quality  trimmed_fasta  assembly  cds   essential_genes  ssu   mytaxa  mytaxa_scan  distances  taxonomy  stats
         ----  ---------  -------------  ------------  -------------  --------  ---   ---------------  ---   ------  -----------  ---------  --------  -----
 P_aeruginosa  -          -              -             -              done      done  done             done  done    done         queued     queued    queued
P_alcaligenes  -          -              -             -              done      done  done             done  done    done         queued     queued    queued
P_fluorescens  -          -              -             -              done      done  done             done  done    done         queued     queued    queued
  P_mendocina  -          -              -             -              done      done  done             done  done    done         queued     queued    queued
     P_putida  -          -              -             -              done      done  done             done  done    done         queued     queued    queued
   P_stutzeri  -          -              -             -              done      done  done             done  done    done         done       done      done
   P_syringae  -          -              -             -              done      done  done             done  done    done         queued     queued    queued
```
Because the datasets submitted were assembled genomes, assembly is the first column to have entries. When all entries under the stats column read "done," processing is finished.

Stop the daemon with the command:

```
miga daemon stop -P .
```

Exit the MiGA singularity container with:

```
exit
```
See the section "Exploting Results" for how to access the results of your project.
