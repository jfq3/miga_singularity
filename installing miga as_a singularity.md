# Installing

Log into the HPCC with your user name and password.  
Create a singularitiy image of MiGa named miga using the comand:

```
singularity build MiGa.simg docker://fyuan277/miga_cli:v0.4t
```
The file extension `simg` is included to indicate that this file is a  source image.  

Once the image is built successfully, run the image shell to start MiGA by entering the command: 

```
singularity shell MiGa
```

MiGa must be initialized the first time the image is run using the command:  

```
miga init
```
When this command is run to initialize MiGa, all dependencies will be configurated automatically except for several optional features and system parameters that need to be manually configured, such as MyTaxa. In this version, MyTaxa is not included because it would make the image size too large. So please type “no” when the system asks if you want to use MyTaxa. Also, please type “bash” when the system asks how to submit jobs. For all other questions, accept the defaults by entering `Enter`.

MiGa is now installed in your home directory. Close your connection to the HPCC.