Here are given the basic steps to install REST-for-physics on the CC IN2P3 cluster.
You can also do it on your personal machine

We need Root and Geant4 (for simulations with restG4) installed for REST-for-physics installation and use.
- Root version 6.26/10 is required
- Geant4 version 11.0.3 is recommended and support will be given with this version. You can decide to use version 10.7 but in case of crash ==we can't give support==.

The first section explains how to install and use Miniconda in order to get the good versions of Root and Geant4.
It allows to create environments enclosing the desired versions, without interfering with the version that you may already have installed for other purposes.
This step is not mandatory, and you can directly go to the [REST-for-physics installation step.](#installing-rest-for-physics)

For any problem with the installation of REST, please see the [REST-for-physics forum](https://rest-forum.unizar.es/), or the [Github page](https://github.com/rest-for-physics).
Also, if you encounter any problem which is not listed in the troubleshooting sections of this document, please report your problem (and possible solution) [here](https://github.com/girardcarillo/REST-guidlines/issues), for improvement of this documentation. 

# Installing Miniconda

Some [helpful documentation](https://docs.conda.io/en/latest/miniconda.html) for Miniconda installation.

1. Get the last version of miniconda

```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```
2. Use the executable

```
sh Miniconda3-latest-Linux-x86_64.sh
``` 
Then follow the instruction on your console

3. You can choose to initiate conda in your shell. This step is not mandatory. 

```
miniconda3/bin/conda init <shell_name>
```

# Creating environment

1. Creating an environment with the right version of Geant4. [Some help](https://github.com/conda-forge/geant4-feedstock) for installing Geant4 with conda.

- For that we need the conda-forge channel

```
conda config --add channels conda-forge
conda config --set channel_priority strict
```

- Now we can create our environement that will eventually contain geant4 and Root.

```
conda create -n <environment_name> geant4==11.0.3
```
This step can take several minutes.

- ==Once it's done you can activate your environement==. This is really important for the installation of Root in the same environement.

```
conda activate <environment_name>
``` 

2. Installing in this environement the right version of Root 

- For Root we need to change the priority of the conda-forge channel. 

```
conda config --set channel_priority false
```

- Now we can install Root

```
conda install root==6.26.10
```

This step can take several minutes, even half an hour.
Time for coffee :coffee:

> It's really important here to precise the version of Root. 
> Otherwise this step can take hours.


## Troubleshooting :boom:

- packages not found
>
```console
PackagesNotFoundError: The following packages are not available from current channels:
- geant4==11.0.3
```
because conda-forge isn't setup as priority channel. Please do
```
conda config --add channels conda-forge
conda config --set channel_priority strict
```

- conda command does not exist
>
```console
command not found: conda
```
You have to set up conda with `conda init <shell>` or create a conda command corresponding to `miniconda3/bin/conda` in the .bashrc or other shell.

# Installing REST-for-physics

Now we have the right versions of Root and Geant4, let's install REST.

- REST [user's guide](https://rest-for-physics.github.io/)
- [Useful slides](https://indico.capa.unizar.es/event/26/timetable/?view=standard) for installation
- [REST-for-physics github page](https://github.com/rest-for-physics)

## Instruction for installing REST at ~/

> Careful: 
> - Please adapt the path to your corresponding repository!
> - If not already done, first activate the conda environement we just created by doing `conda activate <environment_name>` 

1. Clone the Github repository

```
cd ~/
mkdir REST-for-physics
cd REST-for-physics
mkdir rest-framework
cd rest-framework
git clone https://github.com/rest-for-physics/framework.git .
python pull-submodules.py –latest:master
```

At this step probably some submodules will failed.
If only iaxo and detector-template fail, that's normal. 
If other modules fail, that's not a good sign.

2. Then build and install the project 
```
mkdir build
cd build
cmake .. -DCMAKE_INSTALL_PREFIX=~/REST-for-physics/rest-framework/install/master -DREST_ALL_LIBS=ON -DREST_G4=ON -DREST_GARFIELD=OFF -DREST_MPFR=OFF
```

You can check `nproc` at Lyon and use the corresponding number.
```
make <-j20> install
```
If this number is too high you could have the classic
```console
internal compiler error: CPU time limit exceeded signal terminated program cc1plus
```
or you could even be kick out of the cluster.
In this case reconnect if necessary, and just relaunch the `make install` command.

The installation of REST is completed !! :clap:

# Using REST

Know you can source REST by doing
```
source <path_to_REST>/REST-for-physics/rest-framework/install/master/thisREST.sh
```
You can write this line in you .bashrc if needed.

Have fun!

## Troubleshooting :boom:

- At the make step, if there is a crash with the mpfr library, you can install it in the same environement:
```
conda install mpfr
```

- A problem with the cmake version at Lyon

```console
CMake Error at CMakeLists.txt:1 (cmake_minimum_required):
  CMake 3.16 or higher is required.  You are running version 2.8.12.2
```

Install in your conda environement the last version of cmake
```
conda install cmake==3.26.3
```
And then deactivate and activate again your conda environement for the changes to be updated.

You could also choose to update cmake directly at CC Lyon with apt-get.
That depends on the requirements of the other softwares you usually run on the cluster.
