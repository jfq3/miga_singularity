# Installing MiGA using Singularity

Log into the HPCC with your user name and password.

**IMPORTANT:** Installation may not work from all nodes. Initially it worked only from dev-intel18.i, but since the HPCC upgrade was completed, I have installed MiGA from dev-intel16. If you have trouble, try a different node and let John Quensen know your experieince. 

```text
ssh  dev-intel18
```

While in you home directory, create a singularitiy image of MiGA named MiGA using the command:

```text
singularity build MiGA docker://fyuan277/miga-web:v1.3
```

Once the image is successfully built, run the image shell to start MiGA by entering the command:

```text
singularity shell MiGA
```

MiGA must be initialized the first time the image is run using the command:

```text
miga init
```

When this command is run to initialize MiGA, all dependencies will be configurated automatically except for several optional features and system parameters that need to be manually configured, such as MyTaxa. MyTaxa is not includedin this version because it would make the image size too large. So please type “no” when the system asks if you want to use MyTaxa. Also, please type “bash” when the system asks how to submit jobs. For all other questions, accept the defaults by entering `Enter`.

MiGA is now installed in your home directory. Close your connection to the HPCC. You must start a new connection before you can use MiGA.

