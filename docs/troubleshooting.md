# FAQ and Troubleshooting with MVAPICH2 

Based on our experience and feedback we have received from our users,
here we include some of the problems a user may experience and the steps
to resolve them. If you are experiencing any other problem, please feel
free to contact us by sending an email to
<mvapich-discuss@cse.ohio-state.edu>.

MVAPICH2 can be used over eight underlying interfaces, namely
OFA-IB-CH3, OFA-IB-Nemesis, OFA-IWARP-CH3, OFA-RoCE-CH3, TrueScale
(PSM-CH3), Omni-Path (PSM2-CH3), TCP/IP-CH3 and TCP/IP-Nemesis. Based on
the underlying library being utilized, the troubleshooting steps may be
different. We have divided the troubleshooting tips into four sections:
General troubleshooting and Troubleshooting over any one of the five
transport interfaces.

## General Questions and Troubleshooting

### Issues with MVAPICH2 and MPI programs that internally override libc functions

The MPI applications overriding the libc functions (e.g., malloc and
free) to use custom memory allocators when running with MVAPICH2 library
lead to issues in MVAPICH2's memory interception mechanism. To
circumvent this issue, the applications can try the following work
around:

-   Use the LD_PRELOAD environment variable to export the path of the
    MVAPICH2 shared library object (libmpich.so or libmpi.so). eg:
    export LD_PRELOAD=/path/to/libmpi.so

### Issues with MVAPICH2 and Python based MPI programs

Using an application written using Python with MVAPICH2 can potentially
result in the memory registration cache mechanism in MVAPICH2 being
disabled at runtime due to an interaction between the Python memory
allocator and the memory registration cache mechanism in MVAPICH2. We
are working towards resolving this issue. In the mean time, we recommend
that you try the following work arounds:

-   Use the LD_PRELOAD environment variable to export the path of the
    MVAPICH2 shared library object (libmpich.so or libmpi.so). eg:
    export LD_PRELOAD=/path/to/libmpi.so

-   Increase the size of the internal communication buffer being used by
    MVAPICH2 and the switch point between eager and rendezvous protocol
    in MVAPICH2 to a larger value. Please refer to
    Section [1.1.4](#impact-of-disabling-memory-registration-cache-on-application-performance) for more details.

### Issues with MVAPICH2 and Google TCMalloc

Using an application that utilizes the Google TCMalloc library with
MVAPICH2 can potentially result in issues at compile time and/or runtime
due to an interaction between the Google TCMalloc library and the memory
registration cache mechanism in MVAPICH2. We are working towards
resolving this issue. In the mean time, we recommend that you disable
the memory registration cache mechanism in MVAPICH2 to work-around this
issue. MVAPICH2 has the capability to memory disable registration cache
support at configure / build time and also at runtime.

If the issue you are facing is at compile time, then you need to
re-configure the MVAPICH2 library to disable memory registration cache
at configure time using the "--disable-registration-cache" option.
Please refer to
Section [config-gen2](install/#configuring-a-build-for-ofa-ib-ch3ofa-iwarp-ch3ofa-roce-ch3) of the MVAPICH2 userguide for more
details on how to disable registration cache at build time.

If the issue you are facing is at run time, then please re-run your
application after setting\
"MV2_USE_LAZY_MEM_UNREGISTER=0". Please refer to
Section [mv2_use_lazy_mem_unregister](/parameters-nem/#mv2_use_lazy_mem_unregister) of the MVAPICH2 userguide
for more details on how to disable registration cache at run time.

Please refer to
Section [1.1.4](#impact-of-disabling-memory-registration-cache-on-application-performance) for more details on the impact of
disabling memory registration cache on application performance.

### Impact of disabling memory registration cache on application performance 

Whether disabling registration cache will have a negative effect on
application performance depends entirely on the communication pattern of
the application. If the application uses mostly small to medium sized
messages (approximately less than 16 KB), then disabling registration
cache will mostly have no impact on the performance of the application.

However, if the application uses messages of larger size, then there
might be an impact depending on the frequency of communication. If this
is the case, then it might be useful to increase the size of the
internal communication buffer being used by MVAPICH2 (using the
"MV2_VBUF_TOTAL_SIZE" environment variable) and the switch point between
eager and rendezvous protocol in MVAPICH2 (using the
"MV2_IBA_EAGER_THRESHOLD") to a larger value. In this scenario, we
recommend that you set both to the same value (possibly slightly greater
than the median message size being used by your application). Please
refer to
Sections [rdma-iba-eager-threshold](/parameters-nem/#mv2_iba_eager_threshold)
and [vbuf-total-size](/parameters-nem/#mv2_vbuf_total_size) of the userguide for more information
about these two parameters.

### MVAPICH2 failed to register memory with InfiniBand HCA

OFED provides network vendor specific kernel module parameters to
control the size of Memory translation table(MTT) used to map virtual to
physical address. This will limit the amount of physical memory can be
registered with InfiniBand device. The following two parameters are
provided to control the size of this table.

1.  log_num_mtt

2.  log_mtts_per_seg

The amount of memory that can be registered is calculated by

It is recommended to adjust log_num_mtt to allow at least twice the
amount of physical memory on your machine. For example, if a node has 64
GB of memory and a 4 KB page size, log_num_mtt should be set to 24 and
(assuming log_mtts_per_seg is set to 1)

These parameters are set on the mlx4_core module in /etc/modprobe.conf\
0.9

### Invalid Communicators Error

This is a problem which typically occurs due to the presence of multiple
installations of MVAPICH2 on the same set of nodes. The problem is due
to the presence of `mpi.h` other than the one, which is used for
executing the program. This problem can be resolved by making sure that
the `mpi.h` from other installation is not included.

### Are `fork()` and `system()` supported?

`fork()` and `system()` is supported for the OpenFabrics device as long
as the kernel is being used is Linux 2.6.16 or newer. Additionally, the
version of OFED used should be 1.2 or higher. The environment variable
IBV_FORK_SAFE=1 must also be set to enable fork support.

### MPI+OpenMP shows bad performance

MVAPICH2 uses CPU affinity to have better performance for
single-threaded programs. For multi-threaded programs, e.g. MPI+OpenMP,
it may schedule all the threads of a process to run on the same CPU. CPU
affinity should be disabled in this case to solve the problem, i.e. set
`MV2_ENABLE_AFFINITY` to 0. In addition, please read Section
 [advanced_multi_thread](/usage/#running-mvapich2-in-multi-threaded-environments) on using MVAPICH2 in
multi-threaded environments. We also recommend using the
compiler/platform specific run-time options to bind the OpenMP threads
to processors. Please refer to Section
( [advanced_omp_thread_binding](/usage/#compiler-specific-flags-to-enable-openmp-thread-binding)) for more information.

### Error message "No such file or directory\" when using Lustre file system

If you are using ADIO support for Lustre, please make sure of the
following:\
-- Check your Lustre setup\
-- You are able to create, read to and write from files in the Lustre
mounted directory\
-- The directory is mounted on all nodes on which the job is executed\
-- The path to the file is correctly specified\
-- The permissions for the file or directory are correctly specified

### Program segfaults with "File locking failed in ADIOI_Set_lock"

If you are using ADIO support for Lustre, the recent Lustre releases
require an additional mount option to have correct file locks. Please
include the following option with your Lustre mount command: "-o
localflock".\

### Running MPI programs built with gfortran

MPI programs built with gfortran might not appear to run correctly due
to the default output buffering used by gfortran. If it seems there is
an issue with program output, the `GFORTRAN_UNBUFFERED_ALL` variable can
be set to "y" and exported into the environment before using the
`mpiexec` or `mpirun_rsh` command to launch the program, as below:\

Or, if using `mpirun_rsh`, export the environment variable as in the
example:

### How do I obtain MVAPICH2 version and configuration information? 

The `mpiname` application is provided with MVAPICH2 to assist with
determining the MPI library version and related information. The usage
of `mpiname` is as follows:

Print MPI library information. With no OPTION, the output is the same as
-v.

-a print all information

-c print compilers

-d print device

-h display this help and exit

-n print the MPI name

-o print configuration options

-r print release date

-v print library version

### How do I compile my MPI application with static libraries, and not use shared libraries? 

MVAPICH2 is configured to be built with shared-libraries by default. To
link your application to the static version of the library, use the
command below when compiling your application:

### Does MVAPICH2 work across AMD and Intel systems?

Yes, as long as you compile MVAPICH2 and your programs on one of the
systems, either AMD or Intel, and run the same binary across the
systems. MVAPICH2 has platform specific parameters for performance
optimizations and it may not work if you compile MVAPICH2 and your
programs on different systems and try to run the binaries together.

### I want to enable debugging for my build. How do I do this?

We recommend that you enable debugging when you intend to take a look at
back traces of processes in GDB (or other debuggers). You can use the
following configure options to enable debugging:
`–enable-g=dbg –disable-fast`.

Additionally:

-   See parameter `MV2_DEBUG_CORESIZE`
    (section [debug-coresize](/parameters-general/#mv2_debug_coresize)) to enable core dumps.

-   See parameter `MV2_DEBUG_SHOW_BACKTRACE`
    (section [debug-backtrace](/parameters-general/#mv2_debug_show_backtrace)) to show a basic backtrace in case
    of error.

### How can I run my application with a different group ID? 

You can specify a different group id for your MPI application using the
`-sg group` option to `mpirun_rsh`. The following example executes
*a.out* on host1 and host2 using *secondarygroup* as their group id.

## Issues and Failures with Job launchers

### /usr/bin/env: mpispawn: No such file or directory

If `mpirun_rsh` fails with this error message, it was unable to locate a
necessary utility. This can be fixed by ensuring that all MVAPICH2
executables are in the PATH on all nodes. If PATHs cannot be setup as
mentioned, then invoke mpirun_rsh with a path prefix. For example:

### TotalView complains that "The MPI library contains no suitable type definition for struct MPIR_PROCDESC"

Ensure that the MVAPICH2 job launcher mpirun_rsh is compiled with debug
symbols. Details are available in Section
[mpi-tv](/usage/#run-using-totalview-debugger-support).

## Problems Building MVAPICH2

### Unable to convert MPI_SIZEOF_AINT to a hex string

`configure: error: Unable to convert MPI_SIZEOF_AINT to a hex string. This is either because we are building on a very strange platform or there is a bug somewhere in configure.`

This error can be misleading. The problem is often not that you're
building on a strange platform, but that there was some problem running
an executable that made configure have trouble determining the size of a
datatype. The true problem is often that you're trying to link against a
library that is not found in your system's default path for linking at
runtime. Please check that you've properly set LD_LIBRARY_PATH or used
the correct rpath settings in LDFLAGS.

### Cannot Build with the PathScale Compiler

There is a known bug with the PathScale compiler (before version 2.5)
when building MVAPICH2. This problem will be solved in the next major
release of the PathScale compiler. To work around this bug, use the the
"`-LNO:simd=0`" C compiler option. This can be set in the build script
similarly to:

    export CC="pathcc -LNO:simd=0"

Please note the use of double quotes. If you are building MVAPICH2 using
the PathScale compiler (version below 2.5), then you should add "-g" to
your CFLAGS, in order to get around a compiler bug.

### nvlink fatal : Unsupported file type '../lib/.libs/libmpich.so'

There have been recent reports of this issue when using the PGI
compiler. This may be able to be solved by adding "-ta=tesla:nordc" to
your CFLAGS. The following example shows MVAPICH2 being configured with
the proper CPPFLAGS and CFLAGS to get around this issue (Note:
--enable-cuda=basic is optional).

::: small

Example:

:   `./configure –enable-cuda=basic CPPFLAGS="-D__x86_64 -D__align__\(n\)=__attribute__\(\(aligned\(n\)\)\) -D__location__\(a\)=__annotate__\(a\) -DCUDARTAPI=" CFLAGS="-ta=tesla:nordc"`
:::

### Libtool has a problem linking with non-GNU compiler (like PGI)

If you are using a compiler that is not recognized by autoconf as a GNU
compiler, Libtool uses an default library search path to look for shared
objects which is \"/lib /usr/lib /usr/local/lib\". Then, if your
libraries are not in one of these paths, MVAPICH2 may fail to link
properly.

You can work around this issue by adding the following configure flags:

    ./configure \
    lt_cv_sys_lib_search_path_spec="/lib64 /usr/lib64 /usr/local/lib64"   \
    lt_cv_sys_lib_dlsearch_path_spec="/lib64 /usr/lib64 /usr/local/lib64" \
    ... ...

The above example considers that the correct library search path for
your system is \"/lib64 /usr/lib64 /usr/local/lib64\".

## With OFA-IB-CH3 Interface

### Cannot Open HCA

The above error reports that the InfiniBand Adapter is not ready for
communication. Make sure that the drivers are up. This can be done by
executing the following command which gives the path at which drivers
are setup.

### Checking state of IB Link

In order to check the status of the IB link, one of the OFED utilities
can be used: `ibstatus`, `ibv_devinfo`.

### Creation of CQ or QP failure

A possible reason could be inability to pin the memory required. Make
sure the following steps are taken.

1.  In `/etc/security/limits.conf` add the following

        * soft memlock phys_mem_in_KB

2.  After this, add the following to `/etc/init.d/sshd`

        ulimit -l  phys_mem_in_KB

3.  Restart sshd

With some distros, we've found that adding the ulimit -l line to the
sshd init script is no longer necessary. For instance, the following
steps work for our RHEL5 systems.

1.  Add the following lines to `/etc/security/limits.conf`

        * soft memlock unlimited
        * hard memlock unlimited

2.  Restart sshd

### Hang with Multi-rail Configuration

If your system has multiple HCAs per node, ensure that the number of
HCAs per node is the same across all nodes. Otherwise, specify the
correct number of HCAs using the parameter
 [num-hcas](/parameters/#mv2_num_hcas).

If you configure MVAPICH2 with `RDMA_CM` and see this issue, ensure that
the different HCAs on the same node are on different subnets.

### Hang with the HSAM Functionality

HSAM functionality uses multi-pathing mechanism with LMC functionality.
However, some versions of OpenFabrics Drivers (including OpenFabrics
Enterprise Distribution (OFED) 1.1) and using the Up\*/Down\* routing
engine do not configure the routes correctly using the LMC mechanism. We
strongly suggest to upgrade to OFED 1.2, which supports Up\*/Down\*
routing engine and LMC mechanism correctly.

### Failure with Automatic Path Migration

MVAPICH2 (OFA-IB-CH3) provides network fault tolerance with Automatic
Path Migration (APM). However, APM is supported only with OFED 1.2
onwards. With OFED 1.1 and prior versions of OpenFabrics drivers, APM
functionality is not completely supported. Please refer to
Section [mv2-use-apm](/parameters/#mv2_use_apm) and
section [mv2-use-apm-test](/parameters/#mv2_use_apm_test)

### Error opening file

If you configure MVAPICH2 with `RDMA_CM` and see this error, you need to
verify if you have setup up the local IP address to be used by RDMA_CM
in the file `/etc/mv2.conf`. Further, you need to make sure that this
file has the appropriate file read permissions. Please follow
Section [mpi-rdma-cm](/usage/#running-with-rdma-cm-support) for more details on this.

### RDMA CM Address error

If you get this error, please verify that the IP address specified
`/etc/mv2.conf` is correctly specified with the IP address of the device
you plan to use RDMA_CM with.

### RDMA CM Route error

If see this error, you need to check whether the specified network is
working or not.

## With OFA-iWARP-CH3 Interface

### Error opening file

If you configure MVAPICH2 with RDMA_CM and see this error, you need to
verify if you have setup up the local IP address to be used by RDMA_CM
in the file `/etc/mv2.conf`. Further, you need to make sure that this
file has the appropriate file read permissions. Please follow
Section [mpi-iwarp](/usage/#run-with-mpirun_rsh-using-ofa-iwarp-interface) for more details on this.

### RDMA CM Address error

If you get this error, please verify that the IP address specified
`/etc/mv2.conf` is correctly specified with the IP address of the device
you plan to use RDMA_CM with.

### RDMA CM Route error

If see this error, you need to check whether the specified network is
working or not.

## Checkpoint/Restart 

### Failure during Restart

Please make sure the following things for a successful restart:

-   The BLCR modules must be loaded on all the compute nodes and the
    console node before a restart

-   The checkpoint file of MPI job console must be accessible from the
    console node.

-   The corresponding checkpoint files of the MPI processes must be
    accessible from the compute nodes using the same path as when
    checkpoint was taken.

The following things can cause a restart to fail:

-   The job which was checkpointed is not terminated or the some
    processes in that job are not cleaned properly. Usually they will be
    cleaned automatically, otherwise, since the pid can't be used by
    BLCR to restart, it will fail.

-   The processes in the job have opened temporary files and these
    temporary files are removed or not accessible from the nodes where
    the processes are restarted on.

-   If the processes are restarted on different nodes, then all the
    nodes must have the exact same libraries installed. In particular,
    you may be required to disable any "prelinking". Please look at
    <https://upc-bugs.lbl.gov//blcr/doc/html/FAQ.html#prelink> for
    further details.

FAQ regarding Berkeley Lab Checkpoint/Restart (BLCR) can be found at:\
 <http://upc-bugs.lbl.gov/blcr/doc/html/FAQ.html> And the user guide for
BLCR can be found at
 <http://upc-bugs.lbl.gov/blcr/doc/html/BLCR_Users_Guide.html>

If you encounter any problem with the Checkpoint/Restart support, please
feel free to contact us at <mvapich-discuss@cse.ohio-state.edu>.

### Errors related to SHArP with multiple concurrent jobs 

There is a large, yet finite number of jobs that can run in parallel
which takes advantage of SHArP. If this limit is exceeded, one may
observe errors like
`ERROR sharp_get_job_data_len failed: Job not found`. This is a system
limitation.
