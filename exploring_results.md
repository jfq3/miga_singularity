The tutorials include how to retrieve some of the more important results for each case: comparing relationships among genomes, finding clades, and evaluating genomes for completeness and contaimination. To retrieve more detailed information it is helpful to understand MiGA's directory structure. The following sub-directories are included under each project directory:

```
.
├── daemon
│   └── daemon.json
├── data
│   ├── 01.raw_reads
│   ├── 02.trimmed_reads
│   ├── 03.read_quality
│   ├── 04.trimmed_fasta
│   ├── 05.assembly
│   ├── 06.cds
│   ├── 07.annotation
│   │   ├── 01.function
│   │   │   ├── 01.essential
│   │   │   └── 02.ssu
│   │   ├── 02.taxonomy
│   │   │   └── 01.mytaxa
│   │   └── 03.qa
│   │       ├── 01.checkm
│   │       └── 02.mytaxa_scan
│   ├── 08.mapping
│   │   ├── 01.read-ctg
│   │   └── 02.read-gene
│   ├── 09.distances
│   │   ├── 01.haai
│   │   ├── 02.aai
│   │   ├── 03.ani
│   │   ├── 04.ssu
│   │   └── 05.taxonomy
│   ├── 10.clades
│   │   ├── 01.find
│   │   ├── 02.ani
│   │   ├── 03.ogs
│   │   ├── 04.phylogeny
│   │   │   ├── 01.essential
│   │   │   └── 02.core
│   │   └── 05.metadata
│   └── 90.stats
├── metadata
└── miga.project.json
```
The sub-directories under data and beginning with 01 thorugh 10 correspond to MiGA's sequential steps in processing data beginning with raw paired fastq files loaded into 01.raw_reads. Not all steps need to be done. In most of the tutorials, we submitted assembled genomes in fasta format; they were loaded into 05.assembly and processing began there. In the genome assembly exercise, we began by loading already trimmed and quality filtered data into 04.trimmed. The genome projects ended with step 09.distances. Step 10.clades was performed only for the clade project. Each of these sub-directories beginning with 01 through 10 contain results for the associated data processing step.

Some of the results are in text format (sometimes compressed), eg. tables, sequence files, gff files, nwk files.
Others are in pdf format or Rdata format.

```
05.assembly - sample_name.LargeContigs.fna - each genome sequence as a fasta file

06.cds - compressed faa, fna and gff3 files for each genome. gff3 gives position of
         gene in the genome and its orientation. Function is not annotated.

07.annotation - 
    01.function
        01.essential
            genome_name.ess (directory)
                log - completeness, contamination, essential genes with multiple
                    copies, missing essential genes
                proteins.aln - aligned proteins sequences of essential genes
                proteins.tar.gz - compressed unaligned protein sequences of essential
                    genes
            genome_name.ess.faa - essential genes (faa) with TIGRFAMs annotations
            genome_name.json - includes completeness and contamination
        02.ssu
            genome_name.json - statistices on 16S sequences found
            genome_name.ssu.all.fa.gz - all 16S sequences found with position annotations
            genome_name.ssu.fa - one 16S sequence in fasta format
            sample_name.ssu.gff.gz - position and orientation of 16S rRNA gene
    02.taxonomy
        01.mytaxa - mytaxa results (not included in the docker version of MiGA)
    03.qa - mytaxa results (not included int he docker version of MiGA)

08.mapping
    01.read-ctq - empty
    02.read.gene - empty

09.distances
    qG_genome_name_u1.intax.txt - classificationo of query genome against reference
       genomes
    qG_genome_name_u1.aai-medoids.tsv - reference genome at center of aai clade
    qG_genome_name_u1.nwk - nwk tree file showing placement of querry genome among
       reference genomes
    qG_genome_name_u1.nwk.pdf - pdf of the nwk tree
    
    01.haai - miga-project.log - list of reference genomes
            - miga-project.txt.gz - table of hAAI identities among reference genomes
            - miga-project.hist - data for plotting distribution of hueristic aai
                distances
            - miga-project.Rdata - heuristic aai distances that can be read into R
    02.aai  - miga-project.log - list of reference genomes
            - miga-project.txt.gz - table of hAAI to AAI identities among reference
                genomes
            - miga-project.hist - data for plotting distribution of aai distances
            - miga-project.Rdata - aai distances that can be read into R
            
    03.ani  - miga-project.log: list of reference genomes
            - miga-project.txt.gz: table of ANI identites among reference genomes
            - miga-project.hist: data for plotting distribution of ani distances
            - miga-project.Rdata: ani distances that can be read into R
            
    04.ssu  - empty
    
    05.taxonomy - empty? version change? OR:
                - genome_name.aai-medoids.tsv - distances to reference genomes
                - genome_name.intax.txt - genome classification to ranks
                - genome_name.nwk - placement of genome among reference genomes
                - genome_name.nwk.pdf - placement of genome among reference genomes
                
10.clades - clade projects only
    01.find - miga-project.aai90-clades: genomes belonging to each aai90 clade;
                each line is a clade
            - miga-project.ani95-clades: genomes belonging to each ani95 clase;
                each line is a clade
            - miga-project.ani95-medoids: genomes representing the medoid of each clade
            - miga-project.proposed-clades: genomes belonging to each proposed clade;
                each line is a clade
    02.ani  - miga-project.ani.nwk
            - miga-project.class.nwk
            - miga-project.class.tsv
            - miga-project.classif
            - miga-project.dist.rdata
            - miga-project.json
            - miga-project.medoids
            - miga-project.pdf
            - directories miga-project.sc-nn/ where nn is a number
    03.ogs  - 2
            - miga-project.core-pan.pdf
            - miga-project.core-pan.tsv
            - miga-project.json
            - miga-project.ogs
            - miga-project.stats
    04.phylogeny
        01.essential: empty
        02.core: empty
    05.metadata: empty

```

## A Taxonomy Summary Script

Fang Yuan has written a script to summarize, for each genome in the project, the closest matching genome in a refernce database and the distances to it. To use it, move to the roject/data/09.distance/05.taxonomy directory and runthe following comamnds:

```
wget https://github.com/jfq3/Miscellaneous-scripts/raw/master/miga_sumdb.sh
chmod 750 miga_sumdb.sh
./miga_sumdb.sh
```
For the miscellaneous genomes project in these tutorials, this produces:

name | closest | haai | aai | ani
---|---|---|---|---
Acidobacterium_capsulatum | Silvibacterium_bohemicum_GCA_001006305 | 99.9508952380952 | 60.0831465538606
Acinetobacter_baumanii | Pseudomonas_pharmafabricae_GCA_002835605 | 98.7412788461538 | 48.5834215277602
Bacillus_anthracis | Bacillus_flexus_NBRC_15715_GCA_001591565 | 98.9199428571429 | 59.5616830477613
Bacillus_cereus | Bacillus_flexus_NBRC_15715_GCA_001591565 | 98.9702788461539 | 59.9991400823551
Bifidobacterium_bifidum | Bifidobacterium_scardovii_JCM_12489___DSM_13734_GCA_000770985 | 99.7717980769231 | 68.9492871800872
Campylobacter_jejuni | Campylobacter_helveticus_GCA_900176295 | 99.1678666666667 | 66.2667775679713
Cytophaga_hutchinsonii | Nafulsella_turpanensis_ZLM_10_GCA_000346615 | 99.9811470588235 | 50.1443577165562
Gemmatimonas_aurantiaca | Gemmatimonas_phototrophica_NZ_CP011454 | 99.9633431372549 | 68.505494616812
Lacunisphaera_limnophila | Oleiharenicola_lentus_GCA_004118375 | 99.9875247524753 | 68.0251176025193

## Browse the results with MiGA-Web

MiGA was orinally provided as a web-based program, and results were delivered via a web browser. You can do the same thing with results generated on a cluster. To do this, first install the Docker version of MiGA on your computer. Compress the results on the cluster into a tar.gz file. You can do this with either the tar command, or with the MiGA archive command. Download the tar.gz file to your computer and decompress it into the MiGA project directory on your computer. Open MiGA-Web and link to the downloaded project.