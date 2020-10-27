# Classify Reference Genomes Against a Reference Database

There are several ways to classify reference genomes using a reference database. The `classify_wf` command automates several steps including downloading a reference database (if one is not found in th default directory), creating a new project, and controlling the daemon. Depending on the number of genomes and whether or not a reference database is already available, this method can take a significant amount of time and the terminal is not usable while the process is running. Thus it is best to submit such a job to the cluster (see the section on **Submitting Jobs** for an example). Also of note, several steps typically included in processing genomes are skipped.

Alternatively, a reference database can be attached to a project when (or after) creating it and the taxonomy step will be included when the project is run (or re-run). In either of these cases, it is necessary to first obtain the reference database.

## Download a Reference Database

The simplest way to get the latest version of the recommended reference database is with the command:

```
miga download
```
This command downloads the latest TypeMat_Lite database to the directory `~/.miga_db`. This database is based on all available genomes of type material (13,510 as of August 2020). The file is large (20 Gb) and requires decompression before use, so expect this command to take several minutes to complete. 

Alternatively, one may use the `get_db` command which offers more control, as seen from the command's help file: 

```
miga get_db -h
Usage: miga get_db [options]

    -n, --database STRING            Name of the database to download. By default: recommended
        --db-version STRING          Database version to download. By default: latest
    -l, --local-dir PATH             Local directory to store the database. By default: /home/john/.miga_db
        --host STRING                Remote host of the database. By default: ftp://microbial-genomes.org/db
        --list                       List available databases and exit
        --list-versions              List available versions of the database and exit
        --no-overwrite               Exit without downloading if the target database already exists
        --no-progress                Supress progress bars
    -v, --verbose                    Print additional information to STDERR
    -d, --debug INT                  Print debugging information to STDERR (1: debug, 2: trace)
    -h, --help                       Display this screen

```

Thus, use the following command to get the latest version of the smaller Phyla_Lite database, which contains one representative genome per phylum, and install it in `~/.miga_db`:

```
 miga get_db -n Phyla_Lite
```
# Add classification to an existing project

To classify the reference genomes in a previously created project, use the edit command with the m flag to link the project to a reference database  and then start the daemon. For example, to classify the reference genomes in the pseudo project, enter the following commands: 

```
miga edit -P ~/pseudo -m ref_project=$HOME/.miga_db/TypeMat_Lite
miga daemon start -P ~/pseudo  --shutdown_when_done
  
```
Monitor progress in the usual way:

```
miga ls -P . -p
```
After the project finishes, stop the daemon and view the resullts:

```
miga daemon stop -P ~/pseudo_new
miga ls -P . -m tax
```

This should return something like:

```
Singularity> miga ls -P . -m tax
P_alcaligenes   d:Bacteria p:Proteobacteria c:Gammaproteobacteria o:Pseudomonadales f:Pseudomonadaceae
P_fluorescens   d:Bacteria p:Proteobacteria c:Gammaproteobacteria
P_mendocina     d:Bacteria p:Proteobacteria c:Gammaproteobacteria o:Pseudomonadales f:Pseudomonadaceae
P_putida        d:Bacteria p:Proteobacteria c:Gammaproteobacteria o:Pseudomonadales f:Pseudomonadaceae
P_stutzeri      d:Bacteria p:Proteobacteria c:Gammaproteobacteria o:Pseudomonadales
P_syringae      d:Bacteria p:Proteobacteria c:Gammaproteobacteria
P_aeruginosa
```

*P. aeruginosa* was not classified because it was submitted as a query genome. Only reference genomes are classified by this method, and only as far as the p-value for the rank is significant. For example, results for individual reference genomes can be examined: 

```
less ~/pseudo/data/09.distances/05.taxonomy/P_mendocina.intax.txt

Closest relative: Pseudomonas_resinovorans_DSM_21078_GCA_000423545 with AAI: 69.43057022118778.

      Rank  Taxonomy                            P-value               Signif.
      ----  --------                            -------               -------
      root  ?                                   0.0                   ****
    domain  Bacteria                            0.0                   ****
    phylum  Proteobacteria                      0.000182886404681892  ****
     class  Gammaproteobacteria                 0.00112779949553833   ****
     order  Pseudomonadales                     0.00240800432831158   ****
    family  Pseudomonadaceae                    0.0283931143268637    ***
     genus  Pseudomonas                         0.243086512889681     *
   species  Pseudomonas resinovorans            0.867026343262541
subspecies  ?                                   0.958065671459814
   dataset  Pseudomonas resinovorans DSM 21078  0.962988363852502

Significance at p-value below: *0.5, **0.1, ***0.05, ****0.01.

less ~/pseudo/data/09.distances/05.taxonomy/P_fluorescens.intax.txt

Closest relative: Motiliproteus_coralliicola_GCA_003345655 with AAI: 51.454620479439285.

      Rank  Taxonomy                    P-value              Signif.
      ----  --------                    -------              -------
      root  ?                           0.0                  ****
    domain  Bacteria                    0.0                  ****
    phylum  Proteobacteria              0.00069720821140883  ****
     class  Gammaproteobacteria         0.00152945342469485  ****
     order  Oceanospirillales           0.307009231778553    *
    family  Oceanospirillaceae          0.502010196742537
     genus  Motiliproteus               0.79973143284691
   species  Motiliproteus coralliicola  0.989864111047939
subspecies  ?                           0.996810113054369
   dataset  ?                           0.997185087046706

Significance at p-value below: *0.5, **0.1, ***0.05, ****0.01.

```

# Include classification in a new project

Create a new project and add reference genomes to it. Then, **BEFORE** starting the daemon, edit the project to include a link to the reference database. The remaining steps are as above:

```
cd
mkdir pseudo_new
miga new -p ~/pseudo_new -t genomes
cd pseudo_new
miga add -P . -t genome -i assembly ~/miga_genomes/*.fasta -m run_mytaxa_scan=false,run_distances=true
miga edit -P ~/pseudo_new -m ref_project=$HOME/.miga_db/TypeMat_Lite
miga daemon start -P ~/pseudo_new
miga ls -P . -p
miga daemon stop -P ~/pseudo_new
miga ls -P . -m tax
```

