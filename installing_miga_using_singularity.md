# Installing MiGA using Singularity

Log into the HPCC with your user name and password.

**IMPORTANT:** Installation may not work from all nodes. Initially it worked only from dev-intel18.i, but since the HPCC upgrade was completed, I have installed MiGA from dev-intel16. If you have trouble, try a different node and let John Quensen know your experieince. 

```
ssh  dev-intel18
```

While in you home directory, create a singularitiy image of MiGA named MiGA using the command:

```
singularity build MiGA docker://fyuan277/miga-web
```

Once the image is successfully built, run the image shell to start MiGA by entering the command:

```
singularity shell MiGA
```

MiGA must be initialized the first time the image is run using the command:

```
miga init
```

When this command is run to initialize MiGA, all dependencies will be configurated automatically except for several optional features and system parameters that need to be manually configured, such as MyTaxa. MyTaxa is not includedin this version because it would make the image size too large. So please type “no” when the system asks if you want to use MyTaxa. Also, please type “bash” when the system asks how to submit jobs. For all other questions, accept the defaults by entering `Enter`.

MiGA is now installed in your home directory. Close your connection to the HPCC. You must start a new connection before you can use MiGA.

## Optional - Installing Reference Databases

If you want to classify submitted genomes taxonomically, there are two reference databases available for the purpose. If you attach one of these to a project, then every reference genome submitted to the project will be classified.

The two reference databases available are **TypeMat_Lite** and **Phyla_Lite**. The August 2020 version of **TypeMat_Lite** contains over 13,500 reference genomes from type material, and **Phyla Lite** contains reference genomes from all 40 bacterial and archaeal phyla. The following command will install the latest version of **Phyla_Lite** in `~/.miga_db`:

```
miga get_db -n Phyla_Lite
```

Being so much larger, **TypeMat_Lite** takes approximately an hour to install. Thus it is more conviniently installed by submitting a job. See the section **Submitting MiGA Jobs** for instructions.