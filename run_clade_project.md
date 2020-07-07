# Run a Clade Project

After logging in, you will be taken to the Dashboard. Click on **Create new project**.

On the **New project** page, enter a short name for your projectt in the **Path** box. Then select the type of project you want to run. The choices are:

- Mixed: Mixed collection of genomes, metagenomes, and viromes
- Genomes: Collection of genomes
- Clade: Collection of closely-related genomes (ANI >= 90%)
- Metagenomes: Collection of metagenomes and/or viromes

**Note:** These require some further explanation.

For this example project, select **Clade**.

Under the section **Identity Engines**, the recommended settings are already filled in. You may change these to make your analysis run more quickly, but with some loss of sensitivity.

You may add a name, description, and comments for your project in the **Optional information** section.

Click on the blue **Create new MiGA **project**** button at the bottom of the page.

After creating the project, you are taken to a page for uploadinig datasets. Because we want to determine distances among our genomes, we select **Upload new query datasets**. This takes us to another page for uploading genome sequences.

The type of data set is Genome, and the type of input is Assembly in FastA format. Click on **Choose Files** and select the 4 pseudomonad genomes. Scroll down to the bottom of the page and click on the blue button **Upload new data set**.

To start processing, you must turn on the daemon. Click on the green website icon in the upper right, select **Admin console**, **Control and review daemons**, and turn on the daemon for your project. You may do this by first turning on the **Daemons controller** and then turning on the **Daemon** for your project. Alternatively, because this is the only project loaded, you may instead click on **Launch all daemons**.

Click on the green website icon in the upper right again, select **Dashboard**, and then **Query datasets** to see the progress of your analysis. 

**Note:** A red circle with an X in it displayed next to a genome if processing for that genome is complete. 

----
This page does not seem to update automatically, or at least to lag significantly behind actual processing progress. Clicking on *Running* .

*P alcaligenes*. Clicking on it took me to page indicating that the Dataset was inactive. Does this simply mean that it has finished processing? I clicked on the orange button to reactivate it. The screen goes white. I used the browser's back arrow to go back to a screen displaying something for that data set. This occurred again for *P aeruginosa*.

**Note:** Information about the status of the analysis is confusing. On the left I see that of the 4 query datasets all are complete. In the upper right is an indication of "1 ready." Two of the genomes, *P fluorescens* and *P alcaligenes*, have red circles with X's next to them.

---

Wait until processing for all genomes is complete. (This took about 10 minutes on my computer.) **Note:** The title bar still shows "3 ready." This always seems to lag behind the number of genomes that are "complete."

Click on **Projects** in the title bar, and then **Public**. Click on the name of your project. The menu in the middle of the screen gives the results for your project. Try clicking on each, e.g. Project Progress, etc. 
Project stats does not display anything.
The distance matrices are empty (except for a header line).
The Rdata files are empty (except for a header line).
Subclades and Orthologous groups of proteins do not display anything.

You can get information about each genome. click on Complete and then one/each of the genomes.

A message at the top of the page indicates that the "dataset was inactivated by the system due to an excessive number of errors." 
Logs for distances indicates that "this task was attempted 11 times." There are no results other than the log which indicates for each attempt:

```
############
Date: 2020-07-07 13:05:33 +0000
Hostname: 09593973783b
MiGA: /var/lib/gems/2.5.0/gems/miga-base-0.7.6.0
Task: cds
Project: /root/miga-data/test_clade
Dataset: qG_P_putida_u1
------------
Selected codon table: 11
############
Date: 2020-07-07 13:06:31 +0000
Hostname: 09593973783b
MiGA: /var/lib/gems/2.5.0/gems/miga-base-0.7.6.0
Task: essential_genes
Project: /root/miga-data/test_clade
Dataset: qG_P_putida_u1
------------
Temporal directory: /tmp/d20200707-17428-57tgva.
Searching models.
Parsing results.
Extracting sequences.
Done.
############
Date: 2020-07-07 13:06:40 +0000
Hostname: 09593973783b
MiGA: /var/lib/gems/2.5.0/gems/miga-base-0.7.6.0
Task: ssu
Project: /root/miga-data/test_clade
Dataset: qG_P_putida_u1
------------
index file ../../../05.assembly/qG_P_putida_u1.LargeContigs.fna.fai not found, generating...
Reading list.
Filtering FastA.
############
Date: 2020-07-07 13:06:44 +0000
Hostname: 09593973783b
MiGA: /var/lib/gems/2.5.0/gems/miga-base-0.7.6.0
Task: distances
Project: /root/miga-data/test_clade
Dataset: qG_P_putida_u1
------------
Options: {:aai_save_rbm=>"save-rbm", :thr=>1, :haai_p=>"blast+", :aai_p=>"blast+", :ani_p=>"blast+", :distances_checkpoint=>10}
Launching analysis
Launching analysis for query dataset
Exception: Cannot add result, incomplete expected files.

DEBUG: /var/lib/gems/2.5.0/gems/miga-base-0.7.6.0/lib/miga/cli/action/add_result.rb:23:in `perform'
DEBUG: /var/lib/gems/2.5.0/gems/miga-base-0.7.6.0/lib/miga/cli/action.rb:32:in `launch'
DEBUG: /var/lib/gems/2.5.0/gems/miga-base-0.7.6.0/lib/miga/cli.rb:197:in `launch'
DEBUG: /var/lib/gems/2.5.0/gems/miga-base-0.7.6.0/bin/miga:10:in `<main>'
############
Date: 2020-07-07 13:06:46 +0000
Hostname: 09593973783b
MiGA: /var/lib/gems/2.5.0/gems/miga-base-0.7.6.0
Task: distances
Project: /root/miga-data/test_clade
Dataset: qG_P_putida_u1
```
Other headings (Ribosomal RNA (small subunit), Quality (essential genes) and Gene prediction)) allow you to download files in gz or Rdata compressed format. Reports are also available under the first two of these. Under Ribosomal RNA, Feature locations gives the position within the genome of each 16S rRNA gene sequence. Under Quality, Full report gives the number of essential genes found, which have multiple copies, and a list of the missing essential genes.





