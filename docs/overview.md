# Overview of the MVAPICH Project

InfiniBand, Omni-Path, Ethernet/iWARP and RDMA over Converged Ethernet
(RoCE) are emerging as high-performance networking technologies to
deliver low latency and high bandwidth. They are also achieving
widespread acceptance due to their *open standards*.

MVAPICH (pronounced as "em-vah-pich") is an *open-source* MPI software
to exploit the novel features and mechanisms of these networking
technologies and deliver best performance and scalability to MPI
applications. This software is developed in the [Network-Based Computing
Laboratory (NBCL)](http://nowlab.cse.ohio-state.edu), headed by [Prof.
Dhabaleswar K. (DK) Panda](http://www.cse.ohio-state.edu/~panda).

The MVAPICH2 MPI library supports MPI-3 semantics. This *open-source*
MPI software project started in 2001 and a first high-performance
implementation was demonstrated at SuperComputing '02 conference. After
that, this software has been steadily gaining acceptance in the HPC,
InfiniBand, Omni-Path, Ethernet/iWARP and RoCE communities. As of May
2021, more than 3,150 organizations (National Labs, Universities and
Industry) world-wide (in 89 countries) have registered as MVAPICH users
at MVAPICH project web site. There have also been more than 1.36 million
downloads of this software from the MVAPICH project site directly. In
addition, many InfiniBand, Omni-Path, Ethernet/iWARP and RoCE vendors,
server vendors, systems integrators and Linux distributors have been
incorporating MVAPICH2 into their software stacks and distributing it.
MVAPICH2 distribution is available under BSD licensing.

Several InfiniBand systems using MVAPICH2 have obtained positions in the
TOP 500 ranking. The Nov '20 list includes the following systems: 4th,
10,649,600-core (Sunway TaihuLight) at National Supercomputing Center in
Wuxi, China 9th, 448, 448 cores (Frontera) at TACC 14th, 391,680 cores
(ABCI) in Japan 21st, 570,020 cores (Neurion) in South Korea 22nd,
556,104 cores (Oakforest-PACS) in Japan 25th, 367,024 cores (Stampede2)
at TACC and 46th, 241,108-core (Pleiades) at NASA. %90, 76,032-core
(Tsubame 2.5) at Tokyo Institute of Technology

More details on MVAPICH software, users list, mailing lists, sample
performance numbers on a wide range of platforms and interconnects, a
set of OSU benchmarks, related publications, and other InfiniBand-,
RoCE, Omni-Path, and iWARP-related projects (High-Performance Big Data
and High-Performance Deep Learning) can be obtained from our
website:<http://mvapich.cse.ohio-state.edu>.

This document contains necessary information for MVAPICH2 users to
download, install, test, use, tune and troubleshoot MVAPICH2 . We
continuously fix bugs and update update this document as per user
feedback. Therefore, we strongly encourage you to refer to our web page
for updates.

# How to use this User Guide?

This guide is designed to take the user through all the steps involved
in configuring, installing, running and tuning MPI applications over
InfiniBand using MVAPICH2 .

In Section [3](#mvapich2-features) we describe all the features in MVAPICH2 . As
you read through this section, please note our new features (highlighted
as [NEW]) compared to version . Some of these
features are designed in order to optimize specific type of MPI
applications and achieve greater scalability.
Section [\[sec:install\]](#sec:install){reference-type="ref"
reference="sec:install"} describes in detail the configuration and
installation steps. This section enables the user to identify specific
compilation flags which can be used to turn some of the features on or
off. Basic usage of MVAPICH2 is explained in
Section [\[sec:usage\]](#sec:usage){reference-type="ref"
reference="sec:usage"}.
Section [\[sec:advanced_usage\]](#sec:advanced_usage){reference-type="ref"
reference="sec:advanced_usage"} provides instructions for running
MVAPICH2 with some of the advanced features.
Section [\[sec:osubenchmarks\]](#sec:osubenchmarks){reference-type="ref"
reference="sec:osubenchmarks"} describes the usage of the OSU
Benchmarks. In
Section [\[sec:performance-tuning\]](#sec:performance-tuning){reference-type="ref"
reference="sec:performance-tuning"} we suggest some tuning techniques
for multi-thousand node clusters using some of our new features. If you
have any problems using MVAPICH2, please check
Section [\[sec:troubleshooting\]](#sec:troubleshooting){reference-type="ref"
reference="sec:troubleshooting"} where we list some of the common
problems people face. Finally, in Sections
[\[def:mvapich-parameters\]](#def:mvapich-parameters){reference-type="ref"
reference="def:mvapich-parameters"} and
[\[def:mvapich-parameters-nem\]](#def:mvapich-parameters-nem){reference-type="ref"
reference="def:mvapich-parameters-nem"}, we list all important run time
parameters, their default values and a short description.

# MVAPICH2 Features  

MVAPICH2 (MPI-3 over InfiniBand) is an MPI-3 implementation based on
[MPICH](http://www.mpich.org/) ADI3 layer. MVAPICH2 is available as a
single integrated package (with MPICH ). The current release supports
ten different underlying transport interfaces, as shown in
Figure [1](#fig:modules).

![Overview of different available interfaces of the MVAPICH2
library](Img/mv2-interfaces.png)

-   OFA-IB-CH3: This interface supports all InfiniBand compliant devices
    based on the [OpenFabrics](http://www.openfabrics.org) layer. This
    interface has the most features and is most widely used. For
    example, this interface can be used over all Mellanox InfiniBand
    adapters, IBM eHCA adapters and TrueScale adapters.

-   OFA-iWARP-CH3: This interface supports all iWARP compliant devices
    supported by OpenFabrics. For example, this layer supports Chelsio
    T3 adapters with the native iWARP mode.

-   OFA-RoCE-CH3: This interface supports the emerging RoCE (RDMA over
    Converged Ethernet) interface for Mellanox ConnectX-EN adapters with
    10/40GigE switches. It provides support for RoCE v1 and v2.

-   TrueScale (PSM-CH3): This interface provides native support for
    TrueScale adapters from Intel over PSM interface. It provides
    high-performance point-to-point communication for both one-sided and
    two-sided operations.

-   Omni-Path (PSM2-CH3): This interface provides native support for
    Omni-Path adapters from Intel over PSM2 interface. It provides
    high-performance point-to-point communication for both one-sided and
    two-sided operations.

-   Shared-Memory-CH3: This interface provides native shared memory
    support on multi-core platforms where communication is required only
    within a node. Such as SMP-only systems, laptops, etc.

-   TCP/IP-CH3: The standard TCP/IP interface (provided by MPICH) to
    work with a range of network adapters supporting TCP/IP interface.
    This interface can be used with IPoIB (TCP/IP over InfiniBand
    network) support of InfiniBand also. However, it will not deliver
    good performance/scalability as compared to the other interfaces.

-   TCP/IP-Nemesis: The standard TCP/IP interface (provided by MPICH
    Nemesis channel) to work with a range of network adapters supporting
    TCP/IP interface. This interface can be used with IPoIB (TCP/IP over
    InfiniBand network) support of InfiniBand also. However, it will not
    deliver good performance/scalability as compared to the other
    interfaces.

-   Shared-Memory-Nemesis: This interface provides native shared memory
    support on multi-core platforms where communication is required only
    within a node. Such as SMP-only systems, laptops, etc.

-   [(DEPRECATED)] OFA-IB-Nemesis: This interface
    supports all InfiniBand compliant devices based on the OpenFabrics
    layer with the emerging Nemesis channel of the MPICH stack. This
    interface can be used by all Mellanox InfiniBand adapters.

MVAPICH2 is compliant with MPI 3 standard. In addition, MVAPICH2
provides support and optimizations for NVIDIA GPU, multi-threading and
fault-tolerance (Checkpoint-restart, Job-pause-migration-resume). A
complete set of features of MVAPICH2 are indicated below. New features
compared to v2.2 are indicated as [NEW].

-   [NEW] Based on and ABI compatible with MPICH-

-   MPI-3 standard compliance

    -   Nonblocking collectives

    -   Neighborhood collectives

    -   MPI_Comm_split_type support

    -   Support for MPI_Type_create_hindexed_block

    -   [NEW] Enhanced support for MPI_T PVARs and
        CVARs

    -   [NEW] Enhanced performance for Allreduce,
        Reduce_scatter_block, Allgather, Allgatherv through new
        algorithms

    -   [NEW] Improved performance for small message
        collective operations

    -   [NEW] Improved performance of data transfers
        from/to non-contiguous buffers used by user-defined datatypes

    -   Nonblocking communicator duplication routine MPI_Comm_idup (will
        only work for single-threaded programs)

    -   MPI_Comm_create_group support

    -   Support for matched probe functionality

    -   Support for \"Const\" (disabled by default)

-   CH3-level design for scaling to multi-thousand cores with highest
    performance and reduced memory usage.

    -   Support for MPI-3 RMA in OFA-IB-CH3, OFA-IWARP-CH3,
        OFA-RoCE-CH3, TrueScale (PSM-CH3) and Omni-Path (PSM2-CH3)

    -   Support for Omni-Path architecture

        -   Introduction of a new PSM2-CH3 channel for Omni-Path

    -   [NEW] Support for Marvel QEDR RoCE adapters

    -   [NEW] Support for PMIx protocol for SLURM
        and JSM

    -   [NEW] Support for RDMA_CM based multicast
        group creation

    -   Support for OpenPOWER architecture

    -   [NEW] Support IBM POWER9 and POWER8
        architecture

    -   [NEW] Support Microsoft Azure HPC cloud
        platform

    -   [NEW] Support Cavium ARM (ThunderX2) systems

    -   [NEW] Support Intel Skylake architecture

    -   [NEW] Support Intel Cascade Lake
        architecture

    -   [NEW] Support AMD EPYC Rome architecture

        -   [NEW] Enhanced point-to-point and
            collective tuning for AMD ROME processor

    -   [NEW] Support for Broadcom NetXtreme RoCE
        HCA

        -   [NEW] Enhanced inter-node point-to-point
            for Broadcom NetXtreme RoCE HCA

    -   [NEW] Support architecture detection for
        Fujitsu A64fx processor

        -   [NEW] Enhanced point-to-point and
            collective tuning for Fujitsu A64fx processor

    -   [NEW] Support architecture detection for
        Oracle BM.HPC2 cloud shape

        -   [NEW]Enhanced point-to-point tuning for
            Oracle BM.HPC2 cloud shape

    -   [NEW] Support for Intel Knights Landing
        architecture

        -   [NEW] Efficient support for different
            Intel Knight's Landing (KNL) models

        -   Optimized inter-node and intra-node communication

        -   [NEW] Enhance large message intra-node
            performance with CH3-IB-Gen2 channel on Intel Knight's
            Landing

    -   [NEW] Support for executing MPI jobs in
        Singularity

    -   Exposing several performance and control variables to MPI-3
        Tools information interface (MPIT)

        -   Enhanced PVAR support

        -   [NEW] Add multiple MPI_T PVARs and CVARs
            for point-to-point and collective operations

    -   [NEW] Enhance performance of point-to-point
        operations for CH3-Gen2 (InfiniBand), CH3-PSM, and CH3-PSM2
        (Omni- Path) channels

    -   Enable support for multiple MPI initializations

    -   Enhanced performance for small messages

    -   Flexibility to use internal communication buffers of different
        size

    -   Enhanced performance for MPI_Comm_split through new bitonic
        algorithm

    -   Tuning internal communication buffer size for performance

    -   Improve communication performance by removing locks from
        critical path

    -   Enhanced communication performance for small/medium message
        sizes

    -   Reduced memory footprint

    -   [NEW] Multi-rail support for UD-Hybrid
        channel

    -   [NEW] Enhanced performance for UD-Hybrid
        code

    -   Support for InfiniBand hardware UD-Multicast based collectives

    -   [NEW] Gracefully handle any number of HCAs

    -   HugePage support

    -   Integrated Hybrid (UD-RC/XRC) design to get best performance on
        large-scale systems with reduced/constant memory footprint

    -   Support for running with UD only mode

    -   Support for MPI-2 Dynamic Process Management on InfiniBand
        Clusters

    -   eXtended Reliable Connection (XRC) support

        -   Enable XRC by default at configure time

    -   Multiple CQ-based design for Chelsio 10GigE/iWARP

    -   Multi-port support for Chelsio 10GigE/iWARP

    -   Enhanced iWARP design for scalability to higher process count

    -   Support iWARP interoperability between Intel NE020 and Chelsio
        T4 adapters

    -   Support for 3D torus topology with appropriate SL settings

    -   Quality of Service (QoS) support with multiple InfiniBand SL

    -   [NEW] Capability to run MPI jobs across
        multiple InfiniBand subnets

    -   Enabling support for intra-node communications in RoCE mode
        without shared memory

    -   On-demand Connection Management: This feature enables InfiniBand
        connections to be setup dynamically, enhancing the scalability
        of MVAPICH2 on clusters of thousands of nodes.

        -   Support for backing on-demand UD CM information with shared
            memory for minimizing memory footprint

        -   Improved on-demand InfiniBand connection setup

        -   On-demand connection management support with IB CM (RoCE
            Interface)

        -   Native InfiniBand Unreliable Datagram (UD) based
            asynchronous connection management for OpenFabrics-IB
            interface.

        -   RDMA CM based on-demand connection management for
            OpenFabrics-IB and\
            OpenFabrics-iWARP interfaces.

        -   [NEW] Support to automatically detect IP
            address of IB/RoCE interfaces when RDMA CM is enabled
            without relying on mv2.conf file

    -   Message coalescing support to enable reduction of per Queue-pair
        send queues for reduction in memory requirement on large scale
        clusters. This design also increases the small message messaging
        rate significantly. Available for OFA-IB-CH3 interface.

    -   RDMA Read utilized for increased overlap of computation and
        communication for OpenFabrics device. Available for OFA-IB-CH3
        and OFA-IB-iWARP-CH3 interfaces.

    -   Shared Receive Queue (SRQ) with flow control. This design uses
        significantly less memory for MPI library. Available for
        OFA-IB-CH3 interface.

    -   Adaptive RDMA Fast Path with Polling Set for low-latency
        messaging. Available for OFA-IB-CH3 and OFA-iWARP-CH3
        interfaces.

    -   Header caching for low-latency

    -   CH3 shared memory channel for standalone hosts (including
        SMP-only systems and laptops) without any InfiniBand adapters

    -   Unify process affinity support in OFA-IB-CH3, PSM-CH3 and
        PSM2-CH3 channels

    -   Support to enable affinity with asynchronous progress thread

    -   Allow processes to request MPI_THREAD_MULTIPLE when socket or
        NUMA node level affinity is specified

    -   Reorganized HCA-aware process mapping

    -   Dynamic identification of maximum read/atomic operations
        supported by HCA

    -   Enhanced scalability for RDMA-based direct one-sided
        communication with less communication resource. Available for
        OFA-IB-CH3 and OFA-iWARP-CH3 interfaces.

    -   Removed libibumad dependency for building the library

    -   Option to disable signal handler setup

    -   Tuned thresholds for various architectures

    -   Option for selecting non-default gid-index in a loss-less fabric
        setup in RoCE mode

    -   Option to use IP address as a fallback if hostname cannot be
        resolved

    -   [NEW] Improved job-startup performance

    -   [NEW] Gracefully handle RDMA_CM failures
        during job-startup

    -   Enhanced startup time for UD-Hybrid channel

    -   Provided a new runtime variable MV2_HOMOGENEOUS_CLUSTER for
        optimized startup on homogeneous clusters

    -   Improved debug messages and error reporting

    -   Supporting large data transfers ($>$`<!-- -->`2GB)

-   Support for MPI communication from NVIDIA GPU device memory

    -   [NEW] Improved performance for Host buffers
        when CUDA is enabled

    -   [NEW] Add custom API to identify if MVAPICH2
        has in-built CUDA support

    -   Support for MPI_Scan and MPI_Exscan collective operations from
        GPU buffers

    -   Multi-rail support for GPU communication

    -   Support for non-blocking streams in asynchronous CUDA transfers
        for better overlap

    -   Dynamic CUDA initialization. Support GPU device selection after
        MPI_Init

    -   Support for running on heterogeneous clusters with GPU and
        non-GPU nodes

    -   Tunable CUDA kernels for vector datatype processing for GPU
        communication

    -   Optimized sub-array data-type processing for GPU-to-GPU
        communication

    -   Added options to specify CUDA library paths

    -   Efficient vector, hindexed datatype processing on GPU buffers

    -   Tuned MPI performance on Kepler GPUs

    -   Improved intra-node communication with GPU buffers using
        pipelined design

    -   Improved inter-node communication with GPU buffers with
        non-blocking CUDA copies

    -   Improved small message communication performance with CUDA IPC
        design

    -   Improved automatic GPU device selection and CUDA context
        management

    -   Optimal communication channel selection for different GPU
        communication modes (DD, HH and HD) in different configurations
        (intra-IOH a and inter-IOH)

    -   Provided option to use CUDA library call instead of CUDA driver
        to check buffer pointer type

    -   High performance RDMA-based inter-node point-to-point
        communication (GPU-GPU, GPU-Host and Host-GPU)

    -   High performance intra-node point-to-point communication for
        multi-GPU adapters/node (GPU-GPU, GPU-Host and Host-GPU)

    -   Enhanced designs for Alltoall and Allgather collective
        communication from GPU device buffers

    -   Optimized and tuned support for collective communication from
        GPU buffers

    -   Non-contiguous datatype support in point-to-point and collective
        communication from GPU buffers

    -   Updated to sm_20 kernel optimizations for MPI Datatypes

    -   Taking advantage of CUDA IPC (available in CUDA 4.1) in
        intra-node communication for multiple GPU adapters/node

    -   Efficient synchronization mechanism using CUDA Events for
        pipelined device data transfers

-   OFA-IB-Nemesis interface design [(Deprecated)]

    -   OpenFabrics InfiniBand network module support for MPICH Nemesis
        modular design

    -   Optimized adaptive RDMA fast path with Polling Set for
        high-performance inter-node communication

    -   Shared Receive Queue (SRQ) support with flow control, uses
        significantly less memory for MPI library

    -   Header caching for low-latency

    -   Support for additional features (such as hwloc, hierarchical
        collectives, one-sided, multi-threading, etc.), as included in
        the MPICH Nemesis channel

    -   Support of Shared-Memory-Nemesis interface on multi-core
        platforms requiring intra-node communication only (SMP-only
        systems, laptops, etc.)

    -   Support for 3D torus topology with appropriate SL settings

    -   Quality of Service (QoS) support with multiple InfiniBand SL

    -   Automatic inter-node communication parameter tuning based on
        platform and adapter detection

    -   Flexible HCA selection

    -   Checkpoint-Restart support

    -   Run-through stabilization support to handle process failures

    -   Enhancements to handle IB errors gracefully

-   Flexible process manager support

    -   Support for PMI-2 based startup with SLURM

    -   Enhanced startup performance with SLURM

        -   Support for PMIX_Iallgather and PMIX_Ifence

    -   Enhanced startup performance and reduced memory footprint for
        storing InfiniBand end-point information with SLURM

        -   Support for shared memory based PMI operations

    -   [NEW] On-demand connection management for
        PSM-CH3 and PSM2-CH3 channels

    -   [NEW] Support for JSM and Flux resource
        managers

    -   [NEW] Enhanced job-startup performance for
        flux job launcher

    -   [NEW] Improved job startup performance with
        mpirun_rsh

    -   [NEW] Support in mpirun_rsh for using srun
        daemons to launch jobs

    -   [NEW] Support in mpirun_rsh for specifying
        processes per node using '-ppn'

    -   Improved startup performance for TrueScale (PSM-CH3) channel

    -   [NEW] Improved job startup time for
        OFA-IB-CH3, PSM-CH3, and PSM2-CH3

    -   Improved hierarchical job startup performance

    -   Enhanced hierarchical ssh-based robust mpirun_rsh framework to
        work with any interface (CH3 and Nemesis channel-based)
        including OFA-IB-Nemesis, TCP/IP-CH3 and TCP/IP-Nemesis to
        launch jobs on multi-thousand core clusters

    -   Introduced option to export environment variables automatically
        with mpirun_rsh

    -   Support for automatic detection of path to utilities(rsh, ssh,
        xterm, TotalView) used by mpirun_rsh during configuration

    -   Support for launching jobs on heterogeneous networks with
        mpirun_rsh

    -   MPMD job launch capability

    -   Hydra process manager to work with any of the ten interfaces
        (CH3 and Nemesis channel-based) including OFA-IB-CH3,
        OFA-iWARP-CH3, OFA-RoCE-CH3 and TCP/IP-CH3

    -   Improved debug message output in process management and fault
        tolerance functionality

    -   Better handling of process signals and error management in
        mpispawn

    -   Flexibility for process execution with alternate group IDs

    -   Using in-band IB communication with MPD

    -   SLURM integration with mpiexec.mpirun_rsh to use SLURM allocated
        hosts without specifying a hostfile

    -   Support added to automatically use PBS_NODEFILE in Torque and
        PBS environments

    -   Support for suspend/resume functionality with mpirun_rsh
        framework

    -   Exporting local rank, local size, global rank and global size
        through environment variables (both mpirun_rsh and hydra)

-   Support for various job launchers and job schedulers (such as SGE
    and OpenPBS/Torque)

-   Configuration file support (similar to one available in MVAPICH).
    Provides a convenient method for handling all runtime variables
    through a configuration file.

-   Fault-tolerance support

    -   Checkpoint-Restart Support with DMTCP (Distributed MultiThreaded
        CheckPointing)

    -   Enable hierarchical SSH-based startup with Checkpoint-Restart

    -   Enable the use of Hydra launcher with Checkpoint-Restart for
        OFA-IB-CH3 and OFA-IB-Nemesis interfaces

    -   Checkpoint/Restart using LLNL's Scalable Checkpoint/Restart
        Library (SCR)

        -   Support for application-level checkpointing

        -   Support for hierarchical system-level checkpointing

    -   Checkpoint-restart support for application transparent
        systems-level fault tolerance.
        [BLCR-based](http://ftg.lbl.gov/CheckpointRestart/CheckpointRestart.shtml)
        support using OFA-IB-CH3 and OFA-IB-Nemesis interfaces

        -   Scalable Checkpoint-restart with mpirun_rsh framework

        -   Checkpoint-restart with [ Fault-Tolerance Backplane
            (FTB)](http://www.mcs.anl.gov/research/cifts/index.php)
            framework (FTB-CR)

        -   Checkpoint-restart with intra-node shared memory
            (user-level) support

        -   Checkpoint-restart with intra-node shared memory
            (kernel-level with LiMIC2) support

        -   Checkpoint-restart support with pure SMP mode

        -   Allows best performance and scalability with fault-tolerance
            support

        -   Run-through stabilization support to handle process failures
            using OFA-IB-Nemesis interface

        -   Enhancements to handle IB errors gracefully using
            OFA-IB-Nemesis interface

    -   Application-initiated system-level checkpointing is also
        supported. User application can request a whole program
        checkpoint synchronously by calling special MVAPICH2 functions.

        -   Flexible interface to work with different files systems.
            Tested with ext3 (local disk), NFS and PVFS2.

    -   Network-Level fault tolerance with Automatic Path Migration
        (APM) for tolerating intermittent network failures over
        InfiniBand.

    -   Fast Checkpoint-Restart support with aggregation scheme

    -   Job Pause-Migration-Restart Framework for Pro-active
        Fault-Tolerance

        -   Enable signal-triggered (SIGUSR2) migration

    -   Fast process migration using RDMA

    -   Support for new standardized Fault Tolerant Backplane (FTB)
        Events for Checkpoint-Restart and Job Pause-Migration-Restart
        Framework

-   Enhancement to software installation

    -   Revamped Build system

        -   Uses automake instead of simplemake,

        -   Allows for parallel builds (\"make -j8\" and similar)

    -   Full autoconf-based configuration

    -   Automatically detects system architecture and adapter types and
        optimizes MVAPICH2 for any particular installation.

    -   A utility (mpiname) for querying the MVAPICH2 library version
        and configuration information

    -   Automatically builds and installs OSU Benchmarks for end-user
        convenience

-   Optimized intra-node communication support by taking advantage of
    shared-memory communication. Available for all interfaces (IB and
    iWARP).

    -   [NEW] Improve support for large processes
        per node and hugepages on SMP systems

    -   Enhanced intra-node SMP performance

    -   Tuned SMP eager threshold parameters

    -   New shared memory design for enhanced intra-node small message
        performance

    -   [NEW] Enhanced performance for shared-memory
        collectives

    -   Support for single copy intra-node communication using Linux
        supported CMA (Cross Memory Attach)

        -   Enabled by default

        -   [NEW] Give preference to CMA if LiMIC2
            and CMA are enabled at the same time

    -   Kernel-level single-copy intra-node communication solution based
        on LiMIC2

        -   Upgraded to LiMIC2 version 0.5.6 to support unlocked ioctl
            calls

        -   LiMIC2 is designed and developed by jointly by The Ohio
            State University and System Software Laboratory at Konkuk
            University, Korea.

    -   Efficient Buffer Organization for Memory Scalability of
        Intra-node Communication

    -   Multi-core optimized

    -   Adjust shared-memory communication block size at runtime

    -   [NEW] Enhanced intra-node and inter-node
        tuning for PSM-CH3 and PSM2-CH3 channels

    -   [NEW] Added logic to detect heterogeneous
        CPU/HFI configurations in PSM-CH3 and PSM2-CH3 channels

    -   [NEW] support for process placement aware
        HCA selection

    -   Automatic intra-node communication parameter tuning based on
        platform

    -   Efficient connection set-up for multi-core systems

    -   [NEW] Portable Hardware Locality (hwloc v)
        support for defining CPU affinity

    -   [NEW] Portable Hardware Locality (hwloc v)
        support for defining CPU affinity

    -   [NEW] NUMA-aware hybrid binding policy for
        dense numa systems such as AMD EPYC (hwloc v)

    -   [NEW] NUMA-aware hybrid binding policy for
        dense numa systems such as AMD EPYC (hwloc v)

    -   [NEW] Add support to select hwloc v1 and
        hwloc v2 at configure time

    -   [NEW] Efficient CPU binding policies
        (spread, bunch, and scatter) to specify CPU binding per job for
        modern multi-core platforms with SMT support

    -   [NEW] Improved multi-rail selection logic

    -   [NEW] Improved heterogeneity detection logic
        for HCA and CPU

    -   Enhanced support for CPU binding with socket and numanode level
        granularity

    -   Enhance MV2_SHOW_CPU_BINDING to enable display of CPU bindings
        on all nodes

    -   Improve performance of architecture detection

    -   Enhanced process mapping support for multi-threaded MPI
        applications

        -   [NEW] Improve support for process to
            core mapping on many-core systems

            -   New environment variable MV2_HYBRID_BINDING_POLICY for
                multi-threaded MPI and MPI+OpenMP applications

            -   Support 'spread', 'linear', and 'compact' placement of
                threads

            -   Warn user if oversubcription of core is detected

        -   [NEW] Introduce
            MV2_CPU_BINDING_POLICY=hybrid

        -   [NEW] Introduce
            MV2_HYBRID_BINDING_POLICY

        -   [NEW] Introduce MV2_THREADS_PER_PROCESS

    -   Improved usability of process to CPU mapping with support of
        delimiters (',' , '-') in CPU listing

    -   Also allows user-defined CPU binding

    -   Optimized for Bus-based SMP and NUMA-Based SMP systems.

    -   Efficient support for diskless clusters

-   Optimized collective communication operations. Available for
    OFA-IB-CH3, OFA-iWARP-CH3, and OFA-RoCE-CH3 interfaces

    -   [NEW] Enhanced small message performance for
        Alltoallv

    -   [NEW] Support collective offload using
        Mellanox's SHARP for Allreduce and Barrier

    -   [NEW] Support collective offload using
        Mellanox's SHARP for Reduce and Bcast

    -   [NEW] Enhanced tuning framework for Reduce
        and Bcast using SHARP

    -   [NEW] Enhanced collective tuning for
        OpenPOWER (POWER8 and POWER9), Intel Skylake and Cavium ARM
        (ThunderX) systems

    -   [NEW] Enhanced point-to-point and collective
        tuning for AMD EPYC Rome, Frontera\@TACC, Longhorn\@TACC,
        Mayer\@Sandia, Pitzer\@OSC, Catalyst\@EPCC, Summit\@ORNL,
        Lassen\@LLNL, Sierra\@LLNL, Expanse\@SDSC, Ookami\@StonyBrook,
        and bb5\@EPFL systems

    -   [NEW] Enhanced collective tuning for Intel
        Knights Landing and Intel Omni-path

    -   [NEW] Enhance collective tuning for
        Bebop\@ANL, Bridges\@PSC, and Stampede2\@TACC systems

    -   [NEW] Efficient CPU binding policies

    -   Optimized collectives (bcast, reduce, and allreduce) for 4K
        processes

    -   Optimized and tuned blocking and non-blocking collectives for
        OFA-IB-CH3, OFA-IB-Nemesis and TrueScale (PSM-CH3) channels

    -   Enhanced MPI_Bcast, MPI_Reduce, MPI_Scatter, MPI_Gather
        performance

    -   Hardware UD-Multicast based designs for collectives - Bcast,
        Allreduce and Scatter

    -   Intra-node Zero-Copy designs for MPI_Gather collective (using
        LiMIC2)

    -   Enhancements and optimizations for point-to-point designs for
        Broadcast, Allreduce collectives

    -   Improved performance for shared-memory based collectives -
        Broadcast, Barrier, Allreduce, Reduce

    -   Performance improvements in Scatterv and Gatherv collectives for
        CH3 interface

    -   Enhancements and optimizations for collectives (Alltoallv,
        Allgather)

    -   Tuned Bcast, alltoall, Scatter, Allgather, Allgatherv, Reduce,
        Reduce_Scatter, Allreduce collectives

-   Integrated multi-rail communication support. Available for
    OFA-IB-CH3 and OFA-iWARP-CH3 interfaces.

    -   Supports multiple queue pairs per port and multiple ports per
        adapter

    -   Supports multiple adapters

    -   Support to selectively use some or all rails according to user
        specification

    -   Support for both one-sided and point-to-point operations

    -   Reduced stack size of internal threads to dramatically reduce
        memory requirement on multi-rail systems

    -   Dynamic detection of multiple InfiniBand adapters and using
        these by default in multi-rail configurations (OFA-IB-CH3,
        OFA-iWARP-CH3 and OFA-RoCE-CH3 interfaces)

    -   Support for process-to-rail binding policy (bunch, scatter and
        user-defined) in multi-rail configurations (OFA-IB-CH3,
        OFA-iWARP-CH3 and OFA-RoCE-CH3 interfaces)

    -   [NEW] Enhance HCA detection to handle cases
        where node has both IB and RoCE HCAs

    -   [NEW] Add support to auto-detect RoCE HCAs
        and auto-detect GID index

    -   [NEW] Add support to use RoCE/Ethernet and
        InfiniBand HCAs at the same time

-   Support for InfiniBand Quality of Service (QoS) with multiple lanes

-   Multi-threading support. Available for all interfaces (IB and
    iWARP), including TCP/IP.

    -   Enhanced support for multi-threaded applications

    -   [NEW] Add support to enable fork safety in
        MVAPICH2 using environment variable

-   High-performance optimized and scalable support for one-sided
    communication: Put, Get and Accumulate. Supported synchronization
    calls: Fence, Active Target, Passive (lock and unlock). Available
    for all interfaces.

    -   Support for handling very large messages in RMA

    -   Enhanced direct RDMA based designs for MPI_Put and MPI_Get
        operations in OFA-IB-CH3 channel

    -   Optimized communication when using MPI_Win_allocate for
        OFA-IB-CH3 channel

    -   Direct RDMA based One-sided communication support for
        OpenFabrics Gen2-iWARP and RDMA CM (with Gen2-IB)

    -   Shared memory backed Windows for one-sided communication

-   Two modes of communication progress

    -   Polling

    -   Blocking (enables running multiple MPI processes/processor).
        Available for OpenFabrics (IB and iWARP) interfaces.

-   Advanced AVL tree-based Resource-aware registration cache

-   Adaptive number of registration cache entries based on job size

-   Automatic detection and tuning for 24-core Haswell architecture

-   Automatic detection and tuning for 28-core Broadwell architecture

-   Automatic detection and tuning for Intel Knights Landing
    architecture

-   Automatic tuning based on both platform type and network adapter

-   Remove verbs dependency when building the PSM-CH3 and PSM2-CH3
    channels

-   Progress engine optimization for TrueScale (PSM-CH3) interface

-   Improved performance for medium size messages for TrueScale
    (PSM-CH3) channel

-   Multi-core-aware collective support for TrueScale (PSM-CH3) channel

-   Collective optimization for TrueScale (PSM-CH3) channel

-   Memory Hook Support provided by integration with ptmalloc2 library.
    This provides safe release of memory to the Operating System and is
    expected to benefit the memory usage of applications that heavily
    use malloc and free operations.

-   Warn and continue when ptmalloc fails to initialize

-   [NEW] Add support to intercept aligned_alloc in
    ptmalloc

-   Support for TotalView debugger with mpirun_rsh framework

-   [NEW] Remove dependency on underlying
    libibverbs, libibmad, libibumad, and librdmacm libraries using
    dlopen

-   Support for linking Intel Trace Analyzer and Collector

-   Shared library support for existing binary MPI application programs
    to run.

-   Enhanced debugging config options to generate core files and
    back-traces

-   Use of gfortran as the default F77 compiler

-   [NEW] Add support for MPI_REAL16 based reduction
    opertaions for Fortran programs

-   [NEW] Supports AMD Optimizing C/C++ (AOCC)
    compiler v2.1.0

-   [NEW] Enhanced support for SHArP v2.1.0

-   ROMIO Support for MPI-IO.

    -   [NEW] Support for DDN Infinite Memory Engine
        (IME)

    -   Optimized, high-performance ADIO driver for Lustre

-   Single code base for the following platforms (Architecture, OS,
    Compilers, Devices and InfiniBand adapters)

    -   Architecture: Knights Landing, OpenPOWER(POWER8 and POWER9),
        ARM, EM64T, x86_64 and x86

    -   Operating Systems: (tested with) Linux

    -   Compilers: GCC, Intel, PGI, and Open64

        -   [NEW] Support for GCC compiler v11

        -   [NEW] Support for Intel IFX Compiler

    -   Devices: OFA-IB-CH3, OFA-iWARP-CH3, OFA-RoCE-CH3, TrueScale
        (PSM-CH3), Omni-Path (PSM2-CH3), TCP/IP-CH3, OFA-IB-Nemesis and
        TCP/IP-Nemesis

    -   InfiniBand adapters (tested with):

        -   Mellanox InfiniHost adapters (SDR and DDR)

        -   Mellanox ConnectX (DDR and QDR with PCIe2)

        -   Mellanox ConnectX-2 (QDR with PCIe2)

        -   Mellanox ConnectX-3 (FDR with PCIe3)

        -   Mellanox Connect-IB (Dual FDR ports with PCIe3)

        -   Mellanox Connect-4 (EDR with PCIe3)

        -   Mellanox ConnectX-5 (EDR with PCIe3)

        -   Mellanox ConnectX-6 (HDR with PCIe3)

        -   Intel TrueScale adapter (SDR)

        -   Intel TrueScale adapter (DDR and QDR with PCIe2)

    -   Intel Omni-Path adapters (tested with):

        -   Intel Omni-Path adapter (100 Gbps with PCIe3)

    -   10GigE (iWARP and RoCE) adapters:

        -   (tested with) Chelsio T3 and T4 adapter with iWARP support

        -   (tested with) Mellanox ConnectX-EN 10GigE adapter

        -   (tested with) Intel NE020 adapter with iWARP support

    -   40GigE RoCE adapters:

        -   (tested with) Mellanox ConnectX-EN 40GigE adapter

The MVAPICH2 package and the project also includes the following
provisions:

::: itemize
[Public SVN](https://scm.nowlab.cse.ohio-state.edu/svn/mpi/mvapich2/)
access of the code-base

A set of micro-benchmarks (including multi-threading latency test) for
carrying out MPI-level performance evaluation after the installation

Public
[mvapich-discuss](http://mailman.cse.ohio-state.edu/mailman/listinfo/mvapich-discuss)
mailing list for mvapich users to

::: itemize
Ask for help and support from each other and get prompt response

Enable users and developers to contribute patches and enhancements
:::
:::
