# Installation Instructions 

The MVAPICH2 installation process is designed to enable the most widely
utilized features on the target build OS by default. The other
interfaces, as indicated in
Figure [\[fig:modules\]](#fig:modules){reference-type="ref"
reference="fig:modules"}, can also be selected on Linux. This
installation section provides generic instructions for building from a
tarball or our latest sources.

*In order to obtain best performance and scalability while having
flexibility to use a large number of features, the MVAPICH team strongly
recommends the use of following interfaces for different adapters: 1)
OFA-IB-CH3 interface for all Mellanox InfiniBand adapters, 2) TrueScale
(PSM-CH3) interface for all Intel InfiniBand adapters, 3) OFA-RoCE-CH3
interface for all RoCE adapters, 4) OFA-iWARP-CH3 for all iWARP adapters
and 5) Shared-Memory-CH3 for single node SMP system and laptop.*

Please see the appropriate subsection for specific configuration
instructions for the interface-adapter you are targeting.

## Building from a tarball

The MVAPICH2 source code package includes MPICH . All the required files
are present as a single tarball. Download the most recent distribution
tarball from:\
 <http://mvapich.cse.ohio-state.edu/downloads>

Unpack the tarball and use the standard GNU procedure to compile:

We now support parallel make and you can use the -j$<$num threads$>$
option to speed up the build process. You can use the following example
to spawn 4 threads instead of the preceding make step.

\
In order to install a debug build, please use the following
configuration option. *Please note that using debug builds may impact
performance.*

## Obtaining and Building the Source from SVN repository

These instructions assume you have already installed subversion.

The MVAPICH2 SVN repository is available at:\
 <https://scm.nowlab.cse.ohio-state.edu/svn/mpi/mvapich2/>

Please keep in mind the following guidelines before deciding which
version to check out:

-   "tags/" is the exact version released with no updates for bug fixes
    or new features.

    -   To obtain the source code from tags/:

-   "trunk" will contain the latest source code as we enhance and
    improve MVAPICH2. It may contain newer features and bug fixes, but
    is lightly tested.

    -   To obtain the source code from trunk:

The mvapich2 directory under your present working directory contains a
working copy of the MVAPICH2 source code. Now that you have obtained a
copy of the source code, you need to update the files in the source
tree:

This script will generate all of the source and configuration files you
need to build MVAPICH2. You will need `autoconf` version $>=$ 2.67,
`automake` version $>=$ 1.12.3, `libtool` version $>=$ 2.4

## Selecting a Process Manager

MVAPICH2 provides the mpirun_rsh/mpispawn framework from MVAPICH
distribution. Using mpirun_rsh should provide the fastest startup of
your MPI jobs. More details can be found in
Section [\[sec:run-mpirun-rsh\]](#sec:run-mpirun-rsh){reference-type="ref"
reference="sec:run-mpirun-rsh"}. In addition, MVAPICH2 also includes the
Hydra process manager from MPICH-. For more details on using Hydra,
please refer to
Section [\[sec:run-hydra\]](#sec:run-hydra){reference-type="ref"
reference="sec:run-hydra"}.

By default, *mpiexec* uses the Hydra process launcher. Please note that
neither mpirun_rsh, nor Hydra require you to start daemons in advance on
the nodes used for a MPI job. Both mpirun_rsh and Hydra can be used with
any of the eight interfaces of this MVAPICH2 release, as indicated in
Figure [\[fig:modules\]](#fig:modules){reference-type="ref"
reference="fig:modules"}.

### Customizing Commands Used by mpirun_rsh

Usage: ./configure \[OPTION\]\... \[VAR=VALUE\]\...

To assign environment variables (e.g., CC, CFLAGS\...), specify them as
VAR=VALUE. See below for descriptions of some of the useful variables.

  --------------- ---------------------------
  RSH_CMD         path to rsh command
  SSH_CMD         path to ssh command
  ENV_CMD         path to env command
  DBG_CMD         path to debugger command
  XTERM_CMD       path to xterm command
  SHELL_CMD       path to shell command
  TOTALVIEW_CMD   path to totalview command
  --------------- ---------------------------

### Using SLURM 

If you'd like to use SLURM to launch your MPI programs please use the
following configure options.

To configure MVAPICH2 to use PMI-1 support in SLURM:

To configure MVAPICH2 to use PMI-2 support in SLURM:

To configure MVAPICH2 to use PMIx support in SLURM:

### Using SLURM with support for PMI Extensions 

MVAPICH2 automatically detects and uses PMI extensions if available from
the process manager. To build and install SLURM with PMI support, please
follow these steps:

Download the SLURM source tarball for SLURM-15.08.8 from\
 <http://slurm.schedmd.com/download.html>.

Download the patch to add PMI Extensions in SLURM from\
 <http://mvapich.cse.ohio-state.edu/download/mvapich/osu-shmempmi-slurm-15.08.8.patch>.

To configure MVAPICH2 with the modified SLURM, please use:

MVAPICH2 can also be configured with PMIx plugin of SLURM:

Note that --with-pmix should refer to the pmix/install directory that is
used to build SLURM.

Please refer to
Section [\[sec:run-slurm\]](#sec:run-slurm){reference-type="ref"
reference="sec:run-slurm"} for information on how to run MVAPICH2 using
SLURM.

### Using Job Step Manager (JSM) 

MVAPICH2 supports PMIx extensions for JSM. To configure MVAPICH2 with
PMIx plugin of JSM, please use:

Note that --with-pmix should refer to the pmix/install directory that is
used to build JSM.

MVAPICH2 can also use pmi4pmix library to support JSM. It can be
configured as follows:

Please refer to
Section [\[sec:run-jsm\]](#sec:run-jsm){reference-type="ref"
reference="sec:run-jsm"} for information on how to run MVAPICH2 using
the Jsrun launcher.

### Using Flux Resource Manager 

To configure MVAPICH2 with Flux support, please use:

Please refer to
Section [\[sec:run-flux\]](#sec:run-flux){reference-type="ref"
reference="sec:run-flux"} for information on how to run MVAPICH2 using
Flux.

## Configuring a build for OFA-IB-CH3/OFA-iWARP-CH3/OFA-RoCE-CH3 

OpenFabrics (OFA) IB/iWARP/RoCE with the CH3 channel is the default
interface on Linux. It can be explicitly selected by configuring with:

Both static and shared libraries are built by default. In order to build
with static libraries only, configure as follows:

To enable use of the TotalView debugger, the library needs to be
configured in the following manner:

Configuration Options for OpenFabrics IB/iWARP/RoCE

-   Configuring with Shared Libraries

    -   Default: Enabled

    -   Enable: `–enable-shared`

    -   Disable: `–disable-shared`

-   Configuring with TotalView support

    -   Default: Disabled

    -   Enable: `–enable-g=dbg –enable-debuginfo`

-   Path to OpenFabrics Header Files

    -   Default: Your PATH

    -   Specify: `–with-ib-include=path`

-   Path to OpenFabrics Libraries

    -   Default: The systems search path for libraries.

    -   Specify: `–with-ib-libpath=path`

-   Support for Hybrid UD-RC/XRC transports

    -   Default: Disabled

    -   Enable: `–enable-hybrid`

-   Support for RDMA CM

    -   Default: enabled, except when BLCR support is enabled

    -   Disable: `–disable-rdma-cm`

-   Support for RoCE

    -   Default: enabled

-   Registration Cache

    -   Default: enabled

    -   Disable: `–disable-registration-cache`

-   ADIO driver for Lustre:

    -   When compiled with this support, MVAPICH2 will use the optimized
        driver for Lustre. In order to enable this feature, the flag\
        `–enable-romio –with-file-system=lustre`\
        should be passed to `configure` (`–enable-romio` is optional as
        it is enabled by default). You can add support for more file
        systems using\
        `–enable-romio –with-file-system=lustre+nfs+pvfs2`

-   LiMIC2 Support

    -   Default: disabled

    -   Enable:\
        `–with-limic2[=<path to LiMIC2 installation>] –with-limic2-include=<path to LiMIC2 headers> –with-limic2-libpath=<path to LiMIC2 library>`

-   CMA Support

    -   Default: enabled

    -   Disable: `–without-cma`

-   Header Caching

    -   Default: enabled

    -   Disable: `–disable-header-caching`

-   MPI Tools Information Interface (MPI-T) Support

    -   Default: disabled

    -   Enable: `–enable-mpit-pvars`

-   Checkpoint/Restart

    -   Option name: `–enable-ckpt`

    -   Require: Berkeley Lab Checkpoint/Restart (BLCR)

    -   Default: disabled

    The Berkeley Lab Checkpoint/Restart (BLCR) installation is
    automatically detected if installed in the standard location. To
    specify an alternative path to the BLCR installation, you can either
    use:\
    `–with-blcr=<path/to/blcr/installation> `\
    or\
    `–with-blcr-include=<path/to/blcr/headers> –with-blcr-libpath=<path/to/blcr/library>`

-   Checkpoint Aggregation

    -   Option name: `–enable-ckpt-aggregation` or
        `–disable-ckpt-aggregation`

    -   Automatically enable Checkpoint/Restart

    -   Require: Filesystem in Userspace (FUSE)

    -   Default: enabled (if Checkpoint/Restart enabled and FUSE is
        present)

    The Filesystem in Userspace (FUSE) installation is automatically
    detected if installed in the standard location. To specify an
    alternative path to the FUSE installation, you can either use:\
    `–with-fuse=<path/to/fuse/installation> `\
    or\
    `–with-fuse-include=<path/to/fuse/headers> –with-fuse-libpath=<path/to/fuse/library>`

-   Application-Level and Transparent System-Level Checkpointing with
    SCR

    -   Option name: `–with-scr`

    -   Default: disabled

    SCR caches checkpoint data in storage on the compute nodes of a
    Linux cluster to provide a fast, scalable checkpoint / restart
    capability for MPI codes.

-   Process Migration

    -   Option name: `–enable-ckpt-migration`

    -   Automatically enable Checkpoint/Restart

    -   Require: Fault Tolerance Backplane (FTB)

    -   Default: disabled

    The Fault Tolerance Backplane (FTB) installation is automatically
    detected if installed in the standard location. To specify an
    alternative path to the FTB installation, you can either use:\
    `–with-ftb=<path/to/ftb/installation> `\
    or\
    `–with-ftb-include=<path/to/ftb/headers> –with-ftb-libpath=<path/to/ftb/library>`

-   eXtended Reliable Connection

    -   Default: enabled (if OFED installation supports it)

    -   Enable: `–enable-xrc`

    -   Disable: `–disable-xrc`

-   HWLOC Support (Affinity)

    -   Default: enabled

    -   Disable: `–without-hwloc`

-   Support for 64K or greater number of cores

    -   Default: 64K or lower number of cores

    -   Enable: `–with-ch3-rank-bits=32`

## Configuring a build for NVIDIA GPU with OFA-IB-CH3 

This section details the configuration option to enable GPU-GPU
communication with the OFA-IB-CH3 interface of the MVAPICH2 MPI library.
For more options on configuring the OFA-IB-CH3 interface, please refer
to Section [1.4](#subsec:config-gen2){reference-type="ref"
reference="subsec:config-gen2"}.

-   Default: disabled

-   Enable: `–enable-cuda`

The CUDA installation is automatically detected if installed in the
standard location. To specify an alternative path to the CUDA
installation, you can either use:\
`–with-cuda=<path/to/cuda/installation> `\
or\
`–with-cuda-include=<path/to/cuda/include> –with-cuda-libpath=<path/to/cuda/libraries>`

In addition to these we have added the following variables to help
account for libraries being installed in different locations:\
`–with-libcuda=<path/to/directory/containing/libcuda>`\
`–with-libcudart=<path/to/directory/containing/libcudart`

::: small
#### Note:

If using the PGI compiler, you will need to add the following to your
`CPPFLAGS` and `CFLAGS`. You'll also need to use the
`–enable-cuda=basic` configure option to build properly. See the example
below.

Example:

:   `./configure –enable-cuda=basic CPPFLAGS="-D__x86_64 -D__align__\(n\)=__attribute__\(\(aligned\(n\)\)\) -D__location__\(a\)=__annotate__\(a\) -DCUDARTAPI=" CFLAGS="-ta=tesla:nordc"`
:::

## Configuring a build to support running jobs across multiple InfiniBand subnets 

The support for running jobs across multiple subnets in MVAPICH2 can be
enabled at configure time as follows:

MVAPICH2 relies on RDMA_CM module to establish connections with peer
processes. The RDMA_CM modules shipped some older versions of OFED (like
OFED-1.5.4.1), do not have the necessary support to enable communication
across multiple subnets. MVAPICH2 is capable of automatically detecting
such OFED installations at configure time. If the OFED installation
present on the system does not support running across multiple subnets,
the configure step will detect this and exit with an error message.

## Configuring a build for Shared-Memory-CH3 

The default CH3 channel provides native support for shared memory
communication on stand alone multi-core nodes that are not equipped with
InfiniBand adapters. The steps to configure CH3 channel explicitly can
be found in Section  [1.4](#subsec:config-gen2){reference-type="ref"
reference="subsec:config-gen2"}. Dynamic Process Management
([\[subsec:dpm\]](#subsec:dpm){reference-type="ref"
reference="subsec:dpm"}) is currently not supported on stand-alone nodes
without InfiniBand adapters.

## Configuring a build for OFA-IB-Nemesis

[The Nemesis sub-channel for OFA-IB is now
deprecated.] It can be built with:

Both static and shared libraries are built by default. In order to build
with static libraries only, configure as follows:

To enable use of the TotalView debugger, the library needs to be
configured in the following manner:

Configuration options for OFA-IB-Nemesis:

-   Configuring with Shared Libraries

    -   Default: Enabled

    -   Enable: `–enable-shared`

    -   Disable: `–disable-shared`

-   Configuring with TotalView support

    -   Default: Disabled

    -   Enable: `–enable-g=dbg –enable-debuginfo`

-   Path to IB Verbs

    -   Default: System Path

    -   Specify: `–with-ibverbs=<path>` or
        `–with-ibverbs-include=<path>` and `–with-ibverbs-lib=<path>`

-   Registration Cache

    -   Default: enabled

    -   Disable: `–disable-registration-cache`

-   Header Caching

    -   Default: enabled

    -   Disable: `–disable-header-caching`

-   Support for 64K or greater number of cores

    -   Default: 64K or lower number of cores

    -   Enable: `–with-ch3-rank-bits=32`

-   Checkpoint/Restart

    -   Default: disabled

    -   Enable: `–enable-checkpointing` and
        `–with-hydra-ckpointlib=blcr`

    -   Require: Berkeley Lab Checkpoint/Restart (BLCR)

    The Berkeley Lab Checkpoint/Restart (BLCR) installation is
    automatically detected if installed in the standard location. To
    specify an alternative path to the BLCR installation, you can either
    use:\
    `–with-blcr=<path/to/blcr/installation> `\
    or\
    `–with-blcr-include=<path/to/blcr/headers> –with-blcr-libpath=<path/to/blcr/library>`

## Configuring a build for Intel TrueScale (PSM-CH3) 

The TrueScale (PSM-CH3) interface needs to be built to use MVAPICH2 on
Intel TrueScale adapters. It can built with:

Both static and shared libraries are built by default. In order to build
with static libraries only, configure as follows:

To enable use of the TotalView debugger, the library needs to be
configured in the following manner:

Configuration options for Intel TrueScale PSM channel:

-   Configuring with Shared Libraries

    -   Default: Enabled

    -   Enable: `–enable-shared`

    -   Disable: `–disable-shared`

-   Configuring with TotalView support

    -   Default: Disabled

    -   Enable: `–enable-g=dbg  –enable-debuginfo`

-   Path to Intel TrueScale PSM header files

    -   Default: The systems search path for header files

    -   Specify: `–with-psm-include=path`

-   Path to Intel TrueScale PSM library

    -   Default: The systems search path for libraries

    -   Specify: `–with-psm-lib=path`

-   Support for 64K or greater number of cores

    -   Default: 64K or lower number of cores

    -   Enable: `–with-ch3-rank-bits=32`

## Configuring a build for Intel Omni-Path (PSM2-CH3) 

The Omni-Path (PSM2-CH3) interface needs to be built to use MVAPICH2 on
Intel Omni-Path adapters. It can built with:

Both static and shared libraries are built by default. In order to build
with static libraries only, configure as follows:

To enable use of the TotalView debugger, the library needs to be
configured in the following manner:

Configuration options for Intel Omni-Path PSM2 channel:

-   Configuring with Shared Libraries

    -   Default: Enabled

    -   Enable: `–enable-shared`

    -   Disable: `–disable-shared`

-   Configuring with TotalView support

    -   Default: Disabled

    -   Enable: `–enable-g=dbg  –enable-debuginfo`

-   Path to Intel Omni-Path PSM2 header files

    -   Default: The systems search path for header files

    -   Specify: `–with-psm2-include=path`

-   Path to Intel Omni-Path PSM2 library

    -   Default: The systems search path for libraries

    -   Specify: `–with-psm2-lib=path`

-   Support for 64K or greater number of cores

    -   Default: 64K or lower number of cores

    -   Enable: `–with-ch3-rank-bits=32`

## Configuring a build for TCP/IP-Nemesis 

The use of TCP/IP with Nemesis channel requires the following
configuration:

Both static and shared libraries are built by default. In order to build
with static libraries only, configure as follows:

To enable use of the TotalView debugger, the library needs to be
configured in the following manner:

Additional instructions for configuring with TCP/IP-Nemesis can be found
in the MPICH documentation available at:
 <http://www.mcs.anl.gov/research/projects/mpich2/documentation/index.php?s=docs>

-   Configuring with Shared Libraries

    -   Default: Enabled

    -   Enable: `–enable-shared`

    -   Disable: `–disable-shared`

-   Configuring with TotalView support

    -   Default: Disabled

    -   Enable: `–enable-g=dbg –enable-debuginfo`

-   Support for 64K or greater number of cores

    -   Default: 64K or lower number of cores

    -   Enable: `–with-ch3-rank-bits=32`

## Configuring a build for TCP/IP-CH3 

The use of TCP/IP requires the explicit selection of a TCP/IP enabled
channel. The recommended channel is TCP/IP Nemesis (described in Section
 [1.13](#subsec:config-tcpip_nemesis-and-ofa_nemesis){reference-type="ref"
reference="subsec:config-tcpip_nemesis-and-ofa_nemesis"}). The
alternative ch3:sock channel can be selected by configuring with:

Both static and shared libraries are built by default. In order to build
with static libraries only, configure as follows:

To enable use of the TotalView debugger, the library needs to be
configured in the following manner:

-   Configuring with Shared Libraries

    -   Default: Enabled

    -   Enable: `–enable-shared`

    -   Disable: `–disable-shared`

-   Configuring with TotalView support

    -   Default: Disabled

    -   Enable: `–enable-g=dbg –enable-debuginfo`

-   Support for 64K or greater number of cores

    -   Default: 64K or lower number of cores

    -   Enable: `–with-ch3-rank-bits=32`

Additional instructions for configuring with TCP/IP can be found in the
MPICH documentation available at:

 <http://www.mpich.org/documentation/guides/>

## Configuring a build for OFA-IB-Nemesis and TCP/IP Nemesis (unified binary) 

MVAPICH2 supports a unified binary for both OFA and TCP/IP communication
through the Nemesis interface.

In order to configure MVAPICH2 for unified binary support, perform the
following steps:

You can use mpicc as usual to compile MPI applications. In order to run
your application on OFA:

To run your application on TCP/IP:

## Configuring a build for Shared-Memory-Nemesis 

The use of Nemesis shared memory channel requires the following
configuration.

Both static and shared libraries are built by default. In order to build
with static libraries only, configure as follows:

To enable use of the TotalView debugger, the library needs to be
configured in the following manner:

Additional instructions for configuring with Shared-Memory-Nemesis can
be found in the MPICH documentation available at:\
 <http://www.mcs.anl.gov/research/projects/mpich2/documentation/index.php?s=docs>

-   Configuring with Shared Libraries

    -   Default: Enabled

    -   Enable: `–enable-shared`

    -   Disable: `–disable-shared`

-   Configuring with TotalView support

    -   Default: Disabled

    -   Enable: `–enable-g=dbg –enable-debuginfo`

## Configuration and Installation with Singularity 

MVAPICH2 can be configured and installed with Singularity in the
following manner. Note that the following prerequisites must be
fulfilled before this step.

-   Singularity must be installed and operational

-   Singularity image must be created as appropriate

#### Sample Configuration and Installation with Singularity

     # Clone the MVAPICH2 in current directory (on host)
     $ git@scm.mvapich.cse.ohio-state.edu:mvapich2.git
     $ cd mvapich2

     # Build MVAPICH2 in the working directory, 
     # using the tool chain within the container
     $ singularity exec Singularity-Centos-7.img ./autogen.sh
     $ singularity exec Singularity-Centos-7.img ./configure --prefix=/usr/local
     $ singularity exec Singularity-Centos-7.img make

     # Install MVAPICH2 into the container as root and writeable option
     $ sudo singularity exec -w Singularity-Centos-7.img make install

## Installation with Spack

MVAPICH2 can be configured and installed with Spack. For detailed
instruction of installing MVAPICH2 with Spack, please refer to Spack
userguide in:\
 <http://mvapich.cse.ohio-state.edu/userguide/userguide_spack/>
