# Running a Genome Project

For this exercise 6 *Psuedomonas* genomes with the file extension `fasta` should be in the directory `$HOME/miga_genomes/pseudo`. This will be the case if you followed the instructions in the section **Get Example Data**. Otherwise you will have to make adjustments to the commands below.

This exercise takes approximately 30 minutes to run.

Log into the HPCC with your user name and password. From your home directory, start MiGA, create a new project named `pseudo`, and move into the `pseudo` directory:

```
cd
mkdir pseudo
singularity shell MiGA
miga new -P ~/pseudo -t genomes
cd pseudo
```

You can safey ignore any warning about a bind mount. The `-t` argument is needed to specify what kind of project you are running. Acceptable types are :

* `genomes` for genomes in general
* `clade` for closely related genomes

For this example, we are using genomes from isolates.

Add your datasets ending with fasta to the MiGA project. In doing this, turn off mytaxa scan and make sure distances is turned on, as in the command below. There must be no space after the comma\(s\) in the items listed for the -m flag or an error will occur. Here the -t flag specifies the type of genome being added to the project. The -i flag specifies that the datasets being uploaded are assembled genomes.

```
miga add -P . -t genome -i assembly ~/miga_genomes/pseudo/*.fasta -m run_mytaxa_scan=false,run_distances=true
```

Launch the daemon to start MiGA processing your data:

```text
miga daemon start -P . --shutdown-when-done
```

The "shutdown-when-done" argument automatically stops the daemon when processing is complete.

After the job starts, you can display the information about the job:

```text
miga about -P .
```

Also after the job starts, you can list the datasets being processed by entering:

```text
miga ls -P . -i
```

And you can monitor the job progress by entering:

```text
miga ls -P . -p
```

This should give something like:

```text
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

```text
miga daemon stop -P .
```

Exit the MiGA singularity container with:

```text
exit
```

See the section **Exploring Results** for how to access the results of your project.

