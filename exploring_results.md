The tutorials include how to retrieve some of the more important results for each case: comparing relationships among genomes, finding clades, and evaluating genomes for completeness and contaimination. To retrieve more detailed information it is helpful to understand MiGA's directory structure. The following sub-directories are included for each project under the project directory:

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

Some of the results are in text format (sometimes compressed), eg. tables, sequence files, gff files, nwk files. Others are in pdf format or Rdata format. The information in each file is explained in the [MiGA workflow section](https://manual.microbial-genomes.org/part5/workflow) of the MiGA manual, and you may download the files individually using FileZilla or a similar program. 


## A Taxonomy Summary Script

For projects including classification, Fang Yuan has written a script to summarize the results. For each classified genome it determines the closest reference genome, the distances to it, and writest the reulsts to the file summary.csv. To use this script, move to the project/data/09.distance/05.taxonomy directory and run the following comamnds:

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

MiGA was originally provided as a web-based program, and the results were delivered via a web browser. You can view MiGA results generated on a cluster in the same manner if you first install the Docker version of MiGA-Web on your computer.  Steps are as follow:

<ol>

<li> If you have not already done so, install the <a href="https://www.docker.com/products/docker-desktop">Docker Version</a> appropriate to your system (Windows, Mac OS or Linux).</li>

<li>If you have not already done so, install MiGA-Web:</li>

<ul type="a">
<li type="a">Open a terminal and enter the following, one line at a time:</li>
<pre><code>docker pull fyuan277/miga-web:v1.3
docker run -p 9090:3000 -it -v C:/miga-web:/root/miga-data -v db_volume:/miga-web/db --name miga-web fyuan277/miga-web:v1.3 /bin/bash
cd miga-web/
export SECRET_KEY_BASE=`bundle exec rake secret`
bundle exec rails server -e production -b 0.0.0.0 -p 3000 Puma
</code></pre>
<p>The docker run line creates the MiGA project directory as C:/miga-web on your computer. You may use a different directory name if you wish.</p>
<li type="a">Enter control C and close the terminal.</li>
<li type="a">Open a new terminal and enter:</li>
<pre><code>docker stop miga-web</code></pre>
<li type="a">Close the terminal.</li>
</ul>

<li>Compress the results on the cluster: </li>

<ol>
<li type="a">Using tar:</li>
<pre><code>cd project_directory</code>
<code>tar czf project_name.tar.gz .</code></pre>
<li type="a">Using miga archive. With MiGA running on the cluster:</li>
<pre><code>cd project_directory
miga archive -o project_name.tar.gz -P .
</code></pre>
<p>The miga archive option has the advantage that unnecessary files are not included in the archive. Thus the archive file is smaller and the MiGA results take up less drive space on your computer.</p>
</ol>

<li>Download the compressed file to the project directory on your computer and decompress it. The installation commands above created the project directory as C:/miga-web.</li>
<pre><code>tar xzf project_name.tar.gz</code></pre>

<li>Start Miga-Web on your computer.</li>

<ol>
<li type="a">Start Docker desktop.</li>
<li type="a">Open a terminal and enter, one line at a time:</li>
<pre><code>docker start miga-web
docker exec -it miga-web /bin/bash
cd miga-web/
export SECRET_KEY_BASE=`bundle exec rake secret`
bundle exec rails server -e production -b 0.0.0.0 -p 3000 Puma
</code></pre>
<li type="a">Leave the terminal running until you are finished browsing the results (see below).</li>
<li type="a">Open a web browser, enter "localhost:9090" as the URL and log into MiGA-Web.</li>
</ol>

<li>Link the project to MiGA-Web.</li>

<ol>
<li type="a">Go to the Admin console page.</li>
<li type="a">Click on "Link existing projects"</li>
<li type="a">Under the name of the project, click either "Link publicly" or "Link privately." The choice only affects where the project will be listed.</li>
</ol>

<li>Open the project and browse the results. Get explanations for each item; the "Learn more" links take you directly to the appropriate section of the MiGA manual. The download links copy the item to your downloads directory.

<li>After you have finished, end MiGA-Web as above after you installed it:</li>

<ol>
<li type="a">Enter control C into the terminal and close it.</li>
<li type="a">Open a new terminal and enter:</li>
<pre><code>docker stop miga-web</code></pre>
<li type="a">Close the terminal.</li>
</ol>

</ol>
