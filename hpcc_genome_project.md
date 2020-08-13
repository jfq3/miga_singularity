# Running a Genome Project Interactively

Log into the HPCC with your user name and password.  

Create a directory for some example data and put some genomes in it. For example, assuming you start from your home directory, the following commands will place several *Psuedomonas* genomes in the directory `~/miga_genomes`:  

```
mkdir miga_genomes
cd mega_genomes
wget https://github.com/jfq3/data_sets/raw/master/Pseudomonas_genomes/pseudomonads.tar.gz
tar xzf pseudomonads.tar.gz
rm pseudomonads.tar.gz
```
Change the extension for one of the files from fasta to fna. For example, 

```
mv P_aeruginosa.fasta P_aeruginosa.fna
```

We will classify this renamed genome relative to the genomes in this project in the next exercise "Classify a Genome."

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
You can safey ignore any warning about a bind mount. The `-t` argument is needed to specify what kind of project you are running.  Acceptable types are :  

- `genomes` for genomes in general
- `clade` for closely related genomes

For this example, we are using genomes from isolates.  

Add your datasets ending with fasta to the MiGA project. In doing this, turn off mytaxa scan and make sure distances is turned on, as in the command below. There must be no space after the comma(s) in the items listed for the -m flag or an error will occur. Here the -t flag specifies the type of genome being added to the project. The -i flag specifies that the datasets being uploaded are assembled genomes.

```
miga add -P . -t genome -i assembly ~/miga_genomes/*.fasta -m run_mytaxa_scan=false,run_distances=true
```

Launch the daemon to start MiGA processing your data:  

```
miga daemon start -P . --shutdown-when-done
```

The "shutdown-when-done" argument automatically stops the daemon when processing is complete.

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
This should give something like:

```
name           raw_reads  trimmed_reads  read_quality  trimmed_fasta  assembly  cds   essential_genes  ssu   mytaxa  mytaxa_scan  distances  taxonomy  stats
         ----  ---------  -------------  ------------  -------------  --------  ---   ---------------  ---   ------  -----------  ---------  --------  -----
P_alcaligenes  -          -              -             -              done      done  done             done  done    done         queued     queued    queued
P_fluorescens  -          -              -             -              done      done  done             done  done    done         queued     queued    queued
  P_mendocina  -          -              -             -              done      done  done             done  done    done         queued     queued    queued
     P_putida  -          -              -             -              done      done  done             done  done    done         queued     queued    queued
   P_stutzeri  -          -              -             -              done      done  done             done  done    done         done       done      done
   P_syringae  -          -              -             -              done      done  done             done  done    done         queued     queued    queued
```
Because the datasets submitted were assembled genomes, assembly is the first column to have entries. When all entries under the stats column read "done," processing is finished.

If you did not use the "shutdown-when-done" argument when starting the daemon, you can stop it with the command:

```
miga daemon stop -P .
```

Exit the MiGA singularity container with:

```
exit
```
See the section "Exploring Results" for how to access the results of your project.
