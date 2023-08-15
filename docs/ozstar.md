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

Then to load these changes run
```bash
source ~/.bashrc
```
to load these changes (this will be done by default when you log in)

The common pulsar software is now loaded and ready for you to use.

If you do not want to automatically load the common pulsar software or load a specific version instead, you can set up `alias` in your `~/.bashrc` like so:

```bash
# Load latest version of pulsar software
alias psrhome="source /fred/oz002/psrhome/scripts/psrhome.sh"
# Load an older version of the pulsar software
alias psr2023="module use /apps/users/pulsar/milan/gcc-11.3.0/modulefiles; module use /apps/users/pulsar/common/modulefiles; module load psrhome/2023-05"
# Load your python virtual environment
alias venv="ml gcc/11.3.0 openmpi/4.1.4 python/3.10.4; source /dir/to/your/venv/bin/activate"
```
So now when you log in you can run `psrhome` to load pulsar software or `venv` if you want to do some python programming ([here](https://supercomputing.swin.edu.au/docs/2-ozstar/Python.html) is OzSTAR's virtual environment documentation).

### Loading pulsar software with tsch

If you prefer the `tsch` shell (C shell) then add the following to your `~/.cshrc` with your favorite test editor:
```bash
source /fred/oz002/psrhome/scripts/psrhome.csh
```

You can change your shell with `changeShell` command (see the [docs](https://supercomputing.swin.edu.au/docs/1-getting_started/FAQ.html?highlight=shell#how-can-i-change-my-login-shell))

