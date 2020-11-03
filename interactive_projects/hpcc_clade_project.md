# Running a Clade Project

For this exercise 10 *Dehalococcoides* genomes with the file extension `fasta` should be in the directory `$HOME/miga_genomes/dehalo`. This will be the case if you followed the instructions in the section **Get Example Data**. Otherwise you will have to make adjustments to the commands below.

This exercise takes approximately 90 minutes to run interactively. The same execise is included under **Submitting MiGA Jobs** if you would rather run it that way.

Log into the HPCC with your user name and password.

Create a directory for your project in your home directory:

```
cd
mkdir dehalo
```

Still from your home directory, start MiGA, create a new project named `dehalo`, and move into the `dehalo` directory:

```
singularity shell MiGA
miga new -P ~/dehalo -t clade
cd dehalo
```

The `-t` argument is needed to specify what kind of project you are running. Acceptable types are :

* `genomes` for the genomes from isolates
* `clade` for genomes of closely related species

For this example, we are using closely related genomes \(expected ANI &gt;= 90%\), so we choose `clade`.

It is not necessary to add a reference database to a clade project. The genomes submitted will be compared among themelves.

Add your data set to the MiGA project. In doing this, turn off mytaxa scan, as in the command below. Here the -t flag specifies that the data being uploaded are genomes and the -i flag specifies that they are already assembled.

```
miga add -P . -t genome -i assembly ~/miga_genomes/dehalo/*.fna -m run_mytaxa_scan=false
```

Launch the daemon to start MiGA processing your data:

```
miga daemon start -P . --shutdown-when-done
```

The shutdown-when-done argument automatically stops the daemon when processing is complete. After the job starts, you can display the project information with the command:

```
miga about -P .
```

Also after the job starts, you can display the dataset information:

```text
miga ls -P . -i
```

And you can monitor its progress by entering:

```text
miga ls -P . -p
```

which should return something like:

```text
name                                raw_reads  trimmed_reads  read_quality  trimmed_fasta  assembly  cds   essential_genes  ssu   mytaxa  mytaxa_scan  distances  taxonomy  stats
----                                ---------  -------------  ------------  -------------  --------  ---   ---------------  ---   ------  -----------  ---------  --------  -----
  GCF_000011905_1_ASM1190v1_genomic  -          -              -             -              done      done  done             done  done    done         queued     queued    queued
 GCF_000341655_1_ASM34165v1_genomic  -          -              -             -              done      done  done             done  done    done         queued     queued    queued
 GCF_000341695_1_ASM34169v1_genomic  -          -              -             -              done      done  done             done  done    done         done       done      done
 GCF_000830925_1_ASM83092v1_genomic  -          -              -             -              done      done  done             done  done    done         done       done      done
GCF_001010485_1_ASM101048v1_genomic  -          -              -             -              done      done  done             done  done    done         done       done      done
GCF_001547795_1_ASM154779v1_genomic  -          -              -             -              done      done  done             done  done    done         done       done      done
GCF_001610775_1_ASM161077v1_genomic  -          -              -             -              done      done  done             done  done    done         done       done      done
GCF_001889305_1_ASM188930v1_genomic  -          -              -             -              done      done  done             done  done    done         done       done      done
GCF_002007845_1_ASM200784v1_genomic  -          -              -             -              done      done  done             done  done    done         queued     queued    queued
GCF_004684285_1_ASM468428v1_genomic  -          -              -             -              done      done  done             done  done    done         queued     queued    queued
```

When all entries in the stats column read "done," the project is finished.

If you did not use the --shutdown-when-done arguement when starting the daemon, you can stop it with:

```text
miga daemon stop -P .
```

Exit the MiGA singularity containeer with:

```text
exit
```

See the section "Exploring Results" for how to access the results of your project.

