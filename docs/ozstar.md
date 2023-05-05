# OzSTAR Guide

The OzSTAR supercomputer is hosted by Swinburne university.
It contains the original OzSTAR cluster and the new Ngarrgu Tindebeek (NT) cluster.

## Getting an account

To gain an account, you can follow [this guide](https://supercomputing.swin.edu.au/docs/1-getting_started/Accounts.html).

Once you have an account you should be able to join a project.
The following projects relate to MeerKat pulsar astronomy:

 - oz002 - General Swinburne pulsar work and Fast Radio Burst searches.
 - oz005 - The MeerKAT Key Science Project on Pulsar Timing (MeerTime).
 - oz242 - To search for relativistic pulsars in data from the 500m FAST telescope.

Request to join all the projects that relate to your work.


## Loading pulsar software

If you prefer the `bash` shell (the default) then add the following to your `~/.bashrc` with your favorite test editor:
```bash
source /fred/oz002/psrhome/scripts/psrhome.sh
```

or if you prefer the `tsch` shell (C shell) then add the following to your `~/.cshrc` with your favorite test editor:
```bash
source /fred/oz002/psrhome/scripts/psrhome.csh
```

Then to load these changes run
```bash
source ~/.bashrc
```
to load these changes (this will be done by default when you log in)

The common pulsar software is now loaded and ready for you to use.

