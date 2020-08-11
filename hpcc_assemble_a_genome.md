# Running an Assembly Project Interactively

Log into the HPCC with your user name and password. 

Create a directory for your reads and put them in it. For this exercise we will start with trimmed fasta reads. 

```
cd
mkdir miga_cap
cd miga_cap
wget https://github.com/jfq3/data_sets/raw/master/A_capsulatum_reads.fasta.gz
gzip -d A_capsulatum_reads.fasta.gz
```

While MiGA usually takes files compressed with gzip, this particular file cannot be read by MiGA and therefore must be decompressed.

Make a directory for your MiGA assembly project.

```
cd
mkdir test_assembly
```

Still from your home directory, start MiGA, create a new project named `test_assembly`, and move into the project directory:  

```
singularity shell MiGA
miga new -P ~/test_assembly/ -t genomes
cd test_assembly
```

Add your data set to the MiGA project. In doing this, turn off mytaxa scan and distances, as in the command below. MyTaxa is not installed in this version of MiGA, and because this project contains only one genome we cannot calculate distances. Here the -i flag specifies that the dataset being uploaded consists of trimmed reads.

```
miga add -P . -t genome -i trimmed_reads_single  ~/miga_cap/A_capsulatum_reads.fasta -m run_mytaxa_scan=false,run_distances=false
```

For the input flag (-i), supported inputs include:
- raw_reads_single: Single raw reads in a single FastQ file
- raw_reads_paired: Paired raw reads in two FastQ files
- trimmed_reads_single: Single trimmed reads in a single FastA file
- trimmed_reads_paired: Paired trimmed reads in two FastA files
- trimmed_reads_interleaved: Paired trimmed reads in a single FastA file
- assembly: Assembled contigs or scaffolds in FastA format

Launch the daemon to start MiGA processing your data:  

```
miga daemon start -P . --shutdown-when-done

```
The `shutdown-when-done` arguement automatically stops the daemon when processing is finished. 

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
name                raw_reads  trimmed_reads  read_quality  trimmed_fasta  assembly  cds     essential_genes  ssu     mytaxa  mytaxa_scan  distances  taxonomy  stats
----                ---------  -------------  ------------  -------------  --------  ---     ---------------  ---     ------  -----------  ---------  --------  -----
A_capsulatum_reads  -          -              -             done           queued    queued  queued           queued  queued  queued       queued     queued    queued

```
Because the dataset submitted consisted of trimmed reads, trimmed_fasta is the first column to have entries. When all entries under the stats column read "done," processing is finished.

If you did not use the "shutdown-when-done" argument when starting the daemon, you can stop it with the command:

```
miga daemon stop -P .
```

Exit the MiGA singularity container with:

```
exit
```
See the section "Exploring Results" for how to access the results of your project.

