Here are given the basic steps to use REST-for-physics on the CC IN2P3 cluster.
A common version of this framework has been deployed here `/pbs/throng/liquido/REST-for-physics`.

In case you don't have a CCIN2P3 account, you could install REST-for-physics on your personal machine.
For that see the 2nd section.

<!-- # Activating CCIN2P3 Root and Geant4 modules -->

<!-- CCIN2P3 has a working system of [pre-installed environment modules](https://doc.cc.in2p3.fr/en/Software/software/modules.html). -->
<!-- This will probably change, especially with the discussions around Singularity -->
<!-- Development of REST -->
<!-- We do that because we don't know if REST will be chosen -->

<!-- module add Modelisation/geant4/11.1.1  -->

<!-- module add Analysis/root/6.26.10 -->

<!-- You will obtain an output like -->

<!-- Loading Analysis/root/6.26.10 -->
<!-- Loading requirement: Programming_Languages/python/3.9.1 Compilers/gcc/9.3.1 Libraries/xerces/3.2.1 HEP/clhep/2.4.6.2 Compilers/gcc/9.3.1 DataManagement/xrootd/4.8.1 -->
<!-- Unloading dependent: Modelisation/geant4/11.1.1 -->
<!-- Unloading conflict: Compilers/gcc/9.3.1 Libraries/xerces/3.2.1 HEP/clhep/2.4.6.2 Compilers/gcc/9.3.1 -->
<!-- Reloading dependent: Modelisation/geant4/11.1.1 -->

<!-- module add Production/cmake/3.20.2 -->

# Using REST @CCIN2P3

1. Initializing conda

Conda is an open-source package management system and environment management system.
For the moment we will use this tool at CCIN2P3 to all work on common version of Root and Geant4.
This may change, following the recent discussion about Singularity.

Miniconda has already been install here `/pbs/throng/liquido`.
This step allows to set conda in your bashrc.

> REST-for-physics has been deployed @CCIN2P3 using these common version of Root and Geant4.
> Then REST will most probably run correctly only using these versions.
> If you try to run REST with other versions than the one it has been installed with, so guarantee can be given.

To activate the common LiquidO conda environement (containing common version of Root and Geant4):

```
/pbs/throng/liquido/miniconda3/bin/conda init bash
```

This will write the initialization of conda in your .bashrc

It's important to initiate conda in the shell you will use for work.
If, at one point, you decide to change your shell, you will have to run this command again with your new shell. 

You can open a new bash session for the changes to be active.

2. Activating the common conda environement for LiquidO

```
conda activate liquido_install
```

3. Make sure that the good version of Root and Geant4 are being used

```
root-config --version
```
should give you 6.26/10, and
```
geant4-config --version
```
should give you 11.0.3.

4. Acivating the common installation of REST

```
source /pbs/throng/liquido/REST-for-physics/rest-framework/install/master/thisREST.sh
```
There is also a `thisREST.csh` at the same location.


# On your personal machine

This section is for people that would have their own installation of REST-for-physics, for development for example.

We need Root and Geant4 (for simulations with restG4) installed, to install and use REST-for-physics.
- Root version 6.26/10 is required
- Geant4 version 11.0.3 is recommended and support will be given with this version. You can decide to use version 10.7 but in case of crash **we can't guaranty support**.

The first section explains how to install and use Miniconda in order to get the good versions of Root and Geant4.
It allows to create environments enclosing the desired versions, without interfering with the version that you may already have installed for other purposes.
This step is not mandatory, and you can directly go to the [REST-for-physics installation step.](#installing-rest-for-physics)

For any problem with the installation of REST, please see the [REST-for-physics forum](https://rest-forum.unizar.es/), or the [Github page](https://github.com/rest-for-physics).
Also, if you encounter any problem which is not listed in the troubleshooting sections of this document, please report your problem (and possible solution) [here](https://github.com/girardcarillo/REST-guidlines/issues), for improvement of this documentation. 

## Installing Miniconda

Some [helpful documentation](https://docs.conda.io/en/latest/miniconda.html) for Miniconda installation.

1. Get the last version of miniconda

```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```
2. Use the executable

```
sh Miniconda3-latest-Linux-x86_64.sh
``` 
Then follow the instruction on your console.

3. You can choose to initiate conda in your shell. This step is not mandatory. 

```
miniconda3/bin/conda init <shell_name>
```

## Creating environment

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

- **Once it's done you can activate your environement**. This is really important for the installation of Root in the same environement.

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


### Troubleshooting :boom:

- packages not found
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
```console
command not found: conda
```
You have to set up conda with `conda init <shell>` or create a conda command corresponding to `miniconda3/bin/conda` in the .bashrc or other shell.

## Installing REST-for-physics

Now we have the right versions of Root and Geant4, let's install REST.

- REST [user's guide](https://rest-for-physics.github.io/)
- [Useful slides](https://indico.capa.unizar.es/event/26/timetable/?view=standard) for installation
- [REST-for-physics github page](https://github.com/rest-for-physics)

### Instruction for installing REST at ~/

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
make install
```
If you are on your personal machine, you could use `make -j6 install` for example (to be matched with your nproc).
At CCIN2P3, this is forbidden.

> Note that at CCIN2P3, options like `make -j install` or `ninja` are not allowed.

The installation of REST is completed !! :clap:

## Using REST

Know you can source REST by doing
```
source <path_to_REST>/REST-for-physics/rest-framework/install/master/thisREST.sh
```
You can write this line in you .bashrc if needed.

Have fun!

### Troubleshooting :boom:

- At the make step, if there is a crash with the mpfr library, in the style:

```console
cannot find -lmpfr: No such file or directory
collect2: error: ld returned 1 exit status
make[2]: *** [source/libraries/axion/libRestAxion.so] Error 1
make[1]: *** [source/libraries/axion/CMakeFiles/RestAxion.dir/all] Error 2
make: *** [all] Error 2
```
then you can install the mpfr library in the same environement:
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
conda install cmake==3.18.4
```

And then deactivate and activate again your conda environement for the changes to be updated.

You could also choose to update cmake directly with apt-get.
That depends on the requirements of the other softwares you usually run.

- when entering a restRoot session, errors at library loading
```console
 - /pbs/home/g/girardca/REST-for-physics/rest-framework/install/master/lib/libRestAxion.so
cling::DynamicLibraryManager::loadLibrary(): /pbs/home/g/girardca/REST-for-physics/rest-framework/install/master/lib/libRestAxion.so: undefined symbol: _ZN23TRestAxionMagneticFieldC1Ev
```

This could come from a too recent version of cmake.
Try to downgrade (version 3.18.4 should be working).

- Binary files not appearing after installation.
See [this discussion](https://rest-forum.unizar.es/t/macos-installation-issue-missing-files-headers-bin/573/5)

- When trying to simulate with restG4
```console
cling::DynamicLibraryManager::loadLibrary(): libgsl.so.25: cannot open shared object file: No such file or directory
Error in <TInterpreter::TCling::AutoLoad>: failure loading library libMathMore.so for ROOT::Math::GSLIntegrator
```
Probably for some reason the gsl library have not been installed properly. Try
```
conda install -c conda-forge gsl==2.7
```
A priori no need to build or install again REST.
Just try again the `restG4` command
