# Classify a Query Genome Against a Genome Project

For this exercise it is assumed that you have already run the **Running a Genome Project** exercise. The project created in that exercise (`pseudo`) consisted of 6 _Psuedomonas_ genomes submitted as fasta files. A seventh *Psudomonas* genome in the same directory had the extension `fna` so that it would not be included in the project when we added all genome files ending with `fasta`. We will now classify this seventh *Psuedomonas* genome in relation to the genomes already in the project; _i.e._ `pseudo` will serve as our reference genome project in this exercise.

Log into the HPCC with your user name and password if you have not done so already.

Add the dataset ending with `fna` to the MiGA project `pseudo`. Here the -D flag gives the name for the one genome being added. The -t flag specifies the type of genome being added to the project. The --query flag (-q is also acceptable) indicates that the genome being added is a query genome (rather than a reference genome, the default). The -i flag specifies that the dataset being uploaded is an assembled genome.

```
cd
singularity shell MiGA # If not already running.
cd pseudo
miga add -P ~/pseudo -D P_aeruginosa -t genome --query -i assembly ~/miga_genomes/pseudo/P_aeruginosa.fna
```

**Note:** It is possible to add more than one genome to a project in this way, but in that case the -D flag cannot be used. Instead a -r flag giving a regex expression for the extraction of the genome names from the names of the files being submitted must be given. This is beyond the scope of this tutorial.

Launch the daemon to start MiGA processing your data:  

```
miga daemon start -P ~/pseudo --shutdown_when_done
```

The "shutdown-when-done" argument automatically stops the daemon when processing is complete.

After the job starts, you can display the information about the job:  

```
miga about -P ~/pseudo
```
Also after the job starts, you can list the datasets being processed by entering:  

```
miga ls -P ~/pseudo -i 
```

And you can monitor the job progress by entering:  

```
miga ls -P ~/pseudo -p
```
This should give something like:


```
-         name  raw_reads  trimmed_reads  read_quality  trimmed_fasta  assembly  cds   essential_genes  ssu   mytaxa  mytaxa_scan  distances  taxonomy  stats
-         ----  ---------  -------------  ------------  -------------  --------  ---   ---------------  ---   ------  -----------  ---------  --------  -----
P_alcaligenes   -          -              -             -              done      done  done             done  done    done         done       done      done
P_fluorescens   -          -              -             -              done      done  done             done  done    done         done       done      done
  P_mendocina   -          -              -             -              done      done  done             done  done    done         done       done      done
     P_putida   -          -              -             -              done      done  done             done  done    done         done       done      done
   P_stutzeri   -          -              -             -              done      done  done             done  done    done         done       done      done
   P_syringae   -          -              -             -              done      done  done             done  done    done         done       done      done
 P_aeruginosa   -          -              -             -              done      done  done             done  done    done         queued     queued    queued

```


Because the dataset submitted was an assembled genome, assembly is the first column to have an entry. When the entry under the stats column reads "-" rather than "queued" processing is finished.

If you did not use the "shutdown-when-done" argument when starting the daemon, you can stop it with the command:

```
miga daemon stop -P ~/pseudo
```

Exit the MiGA singularity container with:

```
exit
```
After exiting singularity, you may view the classfication results with:

```
less ~/pseudo/data/09.distances/P_aeruginosa.intax.txt

```
which should display: 
```
Closest relative: P_alcaligenes with AAI: 67.73878174613809.

      Rank  Taxonomy  P-value            Signif.
      ----  --------  -------            -------
      root  ?         0.0                ****
    domain  ?         0.0                ****
    phylum  ?         0.0                ****
     class  ?         0.0                ****
     order  ?         0.0                ****
    family  ?         0.0                ****
     genus  ?         0.182469447956174  *
   species  ?         0.457648546144121  *
subspecies  ?         0.49936788874842   *

Significance at p-value below: *0.5, **0.1, ***0.05, ****0.01.

```
This gives *P. alcaligenes* as the closest match among the reference genomes in `pseudo` and indicates that there is a very high probability that the genome being classified is in the same family and maybe (p~0.18) in the same genus.

