# Get Data Files

Data for the examples in this GitBook are available on GitHub at `https://github.com/jfq3/data_sets`. You will have to download them to your computer and then upload them to the cluster and decompress them. Using `wget` to retrieve them directly from GitHub does not work. In order to follow the scripts exactly, you need to arrange the files as in the following directory structure. Otherwise you will need to edit the scripts to point to the data files as you arrange them in your directory.

Change the extension of one of the *Pseudomonas* files from fasta to fna. 

```
$HOME/miga_genomes/
├── a_capsulatum
│   └── A_capsulatum_reads.fasta
├── dehalo
│   ├── GCF_000011905.1_ASM1190v1_genomic.fna
│   ├── GCF_000341655.1_ASM34165v1_genomic.fna
│   ├── GCF_000341695.1_ASM34169v1_genomic.fna
│   ├── GCF_000830925.1_ASM83092v1_genomic.fna
│   ├── GCF_001010485.1_ASM101048v1_genomic.fna
│   ├── GCF_001547795.1_ASM154779v1_genomic.fna
│   ├── GCF_001610775.1_ASM161077v1_genomic.fna
│   ├── GCF_001889305.1_ASM188930v1_genomic.fna
│   ├── GCF_002007845.1_ASM200784v1_genomic.fna
│   └── GCF_004684285.1_ASM468428v1_genomic.fna
├── misc
│   ├── Acidobacterium_capsulatum.fasta
│   ├── Acinetobacter_baumanii.fasta
│   ├── Bacillus_anthracis.fasta
│   ├── Bacillus_cereus.fasta
│   ├── Bifidobacterium_bifidum.fasta
│   ├── Campylobacter_jejuni.fasta
│   ├── Cytophaga_hutchinsonii.fasta
│   ├── Gemmatimonas_aurantiaca.fasta
│   └── Lacunisphaera_limnophila.fasta
├── pseudo
│   ├── P_aeruginosa.fna
│   ├── P_alcaligenes.fasta
│   ├── P_fluorescens.fasta
│   ├── P_mendocina.fasta
│   ├── P_putida.fasta
│   ├── P_stutzeri.fasta
│   └── P_syringae.fasta
```