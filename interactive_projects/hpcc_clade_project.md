# Running a Clade Project

Log into the HPCC with your user name and password.

Create a directory for some example data and put some genomes in it. For example, assuming you start from your home directory, the following commands will place several _Dehalococoides_ genomes in the directory `~/dehalo_genomes`:

```text
mkdir dehalo_genomes
cd dehalo_genomes
wget https://github.com/jfq3/data_sets/raw/master/Dehalococcoides_genomes/dehalococcoides_genomes.tar.gz
tar xzf dehalococcoides_genomes.tar.gz
rm dehalococcoides_genomes.tar.gz
```

Create a directory for your project in your home directory:

```text
cd
mkdir dehalo
```

Still from your home directory, start MiGA, create a new project named `dehalo`, and move into the dehalo directory:

```text
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

```text
miga add -P . -t genome -i assembly ~/dehalo_genomes/*.fna -m run_mytaxa_scan=false
```

Launch the daemon to start MiGA processing your data:

```text
miga daemon start -P . --shutdown-when-done
```

The shutdown-when-done argument automatically stops the daemon when processing is complete. After the job starts, you can display the project information with the command:

```text
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

