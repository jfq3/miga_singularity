# Installing

Log into the HPCC with your user name and password.  
Create a singularitiy image of MiGa named miga using the comand:

```
singularity build MiGA docker://fyuan277/miga_cli:v0.4t
```
The file extension `simg` can be added to the name to indicate that this file is a  source image, but I had better luck not doing so.  

Once the image is built successfully, run the image shell to start MiGA by entering the command: 

```
singularity shell MiGA
```
When I entered this command, I got the following:

WARNING: Bind mount '/mnt/home/quensenj => /mnt/home/quensenj' overlaps container CWD /mnt/home/quensenj, may not be available
FATAL:   container creation failed: mount /proc/self/fd/3->/var/singularity/mnt/session/rootfs error: while mounting image /proc/self/fd/3: failed to find loop device: could not attach image file to loop device: failed to get loop flags for loop device: no such file or directory

The hidden files .miga_rc and .miga_daemon.json were not written to the home directory.

I deleted MiGA from the home directory, index.json and oci-layout from .singularity/cache/oci, and the blob files from .singularity/cache/oci/blobs/sha356. 

I tried rebuilidng with the following command but with the same result as above.  

```
singularity build --fix-perms --force MiGA docker://fyuan277/miga_cli:v0.4t
```
I was not able to go past this point.

MiGA must be initialized the first time the image is run using the command:  

```
miga init
```
When this command is run to initialize MiGA, all dependencies will be configurated automatically except for several optional features and system parameters that need to be manually configured, such as MyTaxa. In this version, MyTaxa is not included because it would make the image size too large. So please type “no” when the system asks if you want to use MyTaxa. Also, please type “bash” when the system asks how to submit jobs. For all other questions, accept the defaults by entering `Enter`.

MiGA is now installed in your home directory. Close your connection to the HPCC.