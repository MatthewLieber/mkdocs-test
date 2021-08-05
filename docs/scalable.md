# Scalability features and Performance Tuning for Large Scale Clusters 

MVAPICH2 provides many different parameters for tuning performance for a
wide variety of platforms and applications. This section deals with
tuning CH3-based interfaces. These parameters can be either compile time
parameters or runtime parameters. Please refer to
Section [1](#sec:performance-tuning){reference-type="ref"
reference="sec:performance-tuning"} for a complete description of all
these parameters. In this section we classify these parameters depending
on what you are tuning for and provide guidelines on how to use them.

## Optimizations for homogeneous clusters

MVAPICH2 internally detects the heterogeneity of the cluster in terms of
processor and network interface type. Set parameter
MV2_HOMOGENEOUS_CLUSTER to 1 to skip this detection, if the user already
knows that cluster is homogeneous.

## Improving Job startup performance

MVAPICH2 has several advanced features to speed up launching jobs on HPC
clusters. There are several launcher-agnostic and launcher-specific
parameters that can be used to get the best job startup performance.
More details about these designs can be obtained from:
<http://mvapich.cse.ohio-state.edu/performance/job-startup/>

### Configuration Options (Launcher-Agnostic)

-   Disabling RDMA_CM

    -   Default: Enabled

    -   Disable: `–disable-rdma-cm`

    -   Disabling RDMA_CM will improve job-startup performance,
        particularly on nodes with large number of cores.

### Runtime Parameters (Launcher-Agnostic)

-   MV2_HOMOGENEOUS_CLUSTER

    -   Default: 0 (Disabled)

    -   Setting MV2_HOMOGENEOUS_CLUSTER to 1 on homogeneous clusters
        will improve startup performance.

-   MV2_ON_DEMAND_THRESHOLD

    -   Default: 64 (OFA-IB-CH3), 16 (OFA-iWARP-CH3)

    -   Should be enabled for fast startup. See
        Section [\[def:mv2-on-demand-threshold\]](#def:mv2-on-demand-threshold){reference-type="ref"
        reference="def:mv2-on-demand-threshold"} for details.

-   MV2_ON_DEMAND_UD_INFO_EXCHANGE

    -   Default: 1 (Enabled)

    -   Setting MV2_ON_DEMAND_UD_INFO_EXCHANGE to 1 will enable
        on-demand Address Handle creation for hybrid mode.

### Enabling Optimizations Specific to mpirun_rsh

-   **MV2_MT_DEGREE**: MVAPICH2 has a scalable job launcher --
    mpirun_rsh which uses a tree based mechanism to spawn processes. The
    degree of this tree is determined dynamically to keep the depth low.
    For large clusters, it might be beneficial to further flatten the
    tree by specifying a higher degree. The degree can be overridden
    with the environment variable MV2_MT_DEGREE
    (see [\[def:mt-degree\]](#def:mt-degree){reference-type="ref"
    reference="def:mt-degree"}).

-   **MV2_FASTSSH_THRESHOLD**: MVAPICH2 can use a faster, hierarchical
    launching mechanism on large clusters. This is enabled manually
    using MV2_FASTSSH_THRESHOLD
    (see [\[def:mv2_fastssh_threshold\]](#def:mv2_fastssh_threshold){reference-type="ref"
    reference="def:mv2_fastssh_threshold"}).

-   **MV2_NPROCS_THRESHOLD**: When the number of processes involved is
    beyond 8k, the mpirun_rsh uses a file-based communication scheme to
    create the hierarchical tree. The default value can be overridden
    with the environment variable MV2_NPROCS_THRESHOLD
    (see [\[def:mv2_nprocs_threshold\]](#def:mv2_nprocs_threshold){reference-type="ref"
    reference="def:mv2_nprocs_threshold"}).

### Enabling Optimizations Specific to SLURM

-   **Using OSU optimized SLURM**: For best performance with SLURM, the
    OSU-optimized PMI2 plugin should be used. This requires applying the
    appropriate patch to SLURM. Please refer to
    Section [\[subsec:config-slurm-pmix\]](#subsec:config-slurm-pmix){reference-type="ref"
    reference="subsec:config-slurm-pmix"} for more details.

-   **Using Default SLURM**: If the SLURM installation cannot be
    modified, the default PMI2 plugin provided by SLURM should be used.
    Please see
    section [\[subsec:config-slurm\]](#subsec:config-slurm){reference-type="ref"
    reference="subsec:config-slurm"} for more details.

## Basic QP Resource Tuning

The following parameters affect memory requirements for each QP.

-   MV2_DEFAULT_MAX_SEND_WQE

-   MV2_DEFAULT_MAX_RECV_WQE

-   MV2_MAX_INLINE_SIZE

MV2_DEFAULT_MAX_SEND_WQE and MV2_DEFAULT_MAX_RECV_WQE control the
maximum number of WQEs per QP and MV2_MAX_INLINE_SIZE controls the
maximum inline size. Reducing the values of these two parameters leads
to less memory consumption. They are especially important for large
scale clusters with a large amount of connections and multiple rails.

These two parameters are run-time adjustable. Please refer to
Sections [\[def:max-send-wqe\]](#def:max-send-wqe){reference-type="ref"
reference="def:max-send-wqe"}
and [\[def:max-inline-size\]](#def:max-inline-size){reference-type="ref"
reference="def:max-inline-size"} for details.

## RDMA Based Point-to-Point Tuning

The following parameters are important in tuning the memory requirements
for adaptive rdma fast path feature.

-   MV2_RDMA_FAST_PATH_BUF_SIZE
    ([\[def:rdma-fast-path-buf-size\]](#def:rdma-fast-path-buf-size){reference-type="ref"
    reference="def:rdma-fast-path-buf-size"})

-   MV2_NUM_RDMA_BUFFER
    ([\[def:num-rdma-buffer\]](#def:num-rdma-buffer){reference-type="ref"
    reference="def:num-rdma-buffer"})

MV2_RDMA_FAST_PATH_BUF_SIZE is the size of each buffer used in RDMA fast
path communication.

MV2_NUM_RDMA_BUFFER is number of buffers used for the RDMA fast path
communication.

On the other hand, the product of MV2_RDMA_FAST_PATH_BUF_SIZE and\
MV2_NUM_RDMA_BUFFER generally is a measure of the amount of memory
registered for eager message passing. These buffers are not shared
across connections.

## Shared Receive Queue (SRQ) Tuning

The main environmental parameters controlling the behavior of the Shared
Receive Queue design are:

-   MV2_SRQ_MAX_SIZE
    ([\[def:viadev-srq-max-size\]](#def:viadev-srq-max-size){reference-type="ref"
    reference="def:viadev-srq-max-size"})

-   MV2_SRQ_SIZE
    ([\[def:viadev-srq-size\]](#def:viadev-srq-size){reference-type="ref"
    reference="def:viadev-srq-size"})

-   MV2_SRQ_LIMIT
    ([\[def:viadev-srq-limit\]](#def:viadev-srq-limit){reference-type="ref"
    reference="def:viadev-srq-limit"})

MV2_SRQ_MAX_SIZE is the maximum size of the Shared Receive Queue
(default 4096). You may increase this to value 8192 if the application
requires very large number of processors. The application will start by
only using MV2_SRQ_SIZE buffers (default 256) and will double this value
on every SRQ limit event(upto MV2_SRQ_MAX_SIZE). For long running
applications this re-size should show little effect. If needed, the
MV2_SRQ_SIZE can be increased to 1024 or higher as needed for
applications.\
MV2_SRQ_LIMIT defines the low water-mark for the flow control handler.
This can be reduced if your aim is to reduce the number of interrupts.

## eXtended Reliable Connection (XRC)

MVAPICH2 now supports the eXtended Reliable Connection (XRC) transport
available in recent Mellanox HCAs. This transport helps reduce the
number of QPs needed on multi-core systems. Set MV2_USE_XRC
([\[def:mv2_use_xrc\]](#def:mv2_use_xrc){reference-type="ref"
reference="def:mv2_use_xrc"}) to use XRC with MVAPICH2.

## Shared Memory Tuning

MVAPICH2 uses shared memory communication channel to achieve
high-performance message passing among processes that are on the same
physical node. The two main parameters which are used for tuning shared
memory performance for small messages are SMP_QUEUE_LENGTH
(Section [\[def:smp-queue-length\]](#def:smp-queue-length){reference-type="ref"
reference="def:smp-queue-length"}), and SMP_EAGER_SIZE
(Section [\[def:smp-eagersize\]](#def:smp-eagersize){reference-type="ref"
reference="def:smp-eagersize"}). The two main parameters which are used
for tuning shared memory performance for large messages are
SMP_SEND_BUF_SIZE
(Section [\[def:smp-send-buf-size\]](#def:smp-send-buf-size){reference-type="ref"
reference="def:smp-send-buf-size"}) and\
SMP_NUM_SEND_BUFFER
(Section [\[def:smp-num-send-buffer\]](#def:smp-num-send-buffer){reference-type="ref"
reference="def:smp-num-send-buffer"}).

SMP_QUEUE_LENGTH is the size of the shared memory buffer which is used
to store outstanding small and control messages. SMP_EAGER_SIZE defines
the switch point from Eager protocol to Rendezvous protocol.

Messages larger than SMP_EAGER_SIZE are packetized and sent out in a
pipelined manner.\
SMP_SEND_BUF_SIZE is the packet size, i.e. the send buffer size.
SMP_NUM_SEND_BUFFER is the number of send buffers.

## On-demand Connection Management Tuning

MVAPICH2 uses on-demand connection management to reduce the memory usage
of MPI library. There are 4 parameters to tune connection manager:
MV2_ON_DEMAND_THRESHOLD
(Section [\[def:mv2-on-demand-threshold\]](#def:mv2-on-demand-threshold){reference-type="ref"
reference="def:mv2-on-demand-threshold"}),\
MV2_CM_RECV_BUFFERS
(Section [\[def:mv2-cm-recv-buffers\]](#def:mv2-cm-recv-buffers){reference-type="ref"
reference="def:mv2-cm-recv-buffers"}), MV2_CM_TIMEOUT
(Section [\[def:mv2-cm-timeout\]](#def:mv2-cm-timeout){reference-type="ref"
reference="def:mv2-cm-timeout"}), and\
MV2_CM_SPIN_COUNT
(Section [\[def:mv2-cm-spin-count\]](#def:mv2-cm-spin-count){reference-type="ref"
reference="def:mv2-cm-spin-count"}). The first one applies to OFA-IB-CH3
and OFA-iWARP-CH3 interfaces and the other three only apply to
OFA-IB-CH3 interface.

MV2_ON_DEMAND_THRESHOLD defines threshold for enabling on-demand
connection management scheme. When the size of the job is larger than
the threshold value, on-demand connection management will be used.

MV2_CM_RECV_BUFFERS defines the number of buffers used by connection
manager to establish new connections. These buffers are quite small and
are shared for all connections, so this value may be increased to 8192
for large clusters to avoid retries in case of packet drops.

MV2_CM_TIMEOUT is the timeout value associated with connection
management messages via UD channel. Decreasing this value may lead to
faster retries but at the cost of generating duplicate messages.

MV2_CM_SPIN_COUNT is the number of the connection manager polls for new
control messages from UD channel for each interrupt. This may be
increased to reduce the interrupt overhead when many incoming control
messages from UD channel at the same time.

## Scalable Collectives Tuning

MVAPICH2 uses shared memory to optimize the performance for many
collective operations: MPI_Allreduce, MPI_Reduce, MPI_Barrier, and
MPI_Bcast.

We use shared-memory based collective for most small and medium sized
messages and fall back to the default point-to-point based algorithms
for very large messages. The upper-limits for shared-memory based
collectives are tunable parameters that are specific to each collective
operation. We have variables such as MV2_SHMEM_ALLREDUCE_MSG
([\[def:mv2-shmem-coll-allreduce-threshold\]](#def:mv2-shmem-coll-allreduce-threshold){reference-type="ref"
reference="def:mv2-shmem-coll-allreduce-threshold"}),
MV2_SHMEM_REDUCE_MSG
([\[def:mv2-shmem-coll-reduce-threshold\]](#def:mv2-shmem-coll-reduce-threshold){reference-type="ref"
reference="def:mv2-shmem-coll-reduce-threshold"}) and\
MV2_SHMEM_BCAST_MSG
([\[def:mv2-shmem-coll-bcast-threshold\]](#def:mv2-shmem-coll-bcast-threshold){reference-type="ref"
reference="def:mv2-shmem-coll-bcast-threshold"}), for MPI_Allreduce,
MPI_Reduce and MPI_Bcast collective operations. The default values for
these variables have been set through experimental analysis on some of
our clusters and a few large scale clusters, such as the TACC Ranger.
Users can choose to set these variables at job-launch time to tune the
collective algorithms on different platforms.

### Optimizations for MPI_Bcast

MVAPICH2 supports a 2-level point-to-point tree-based "Knomial"
algorithm for small messages for the MPI_Bcast operation. MVAPICH2 also
offers improved designs that deliver better performance for medium and
large message lengths.

### Optimizations for MPI_Reduce and MPI_Allreduce

In this release, we have introduced new 2-level algorithms for
MPI_Reduce and MPI_Allreduce operations, along the same lines as
MPI_Bcast. Pure Shared-memory based algorithms cannot be used for larger
messages for reduction operations, because the node-leader processes
become the bottleneck, as they have to perform the reduction operation
on the entire data block. We now rely on shared-memory algorithms for
MPI_Reduce and MPI_Allreduce for message sizes set by the thresholds
MV2_SHMEM_ALLREDUCE_MSG
([\[def:mv2-shmem-coll-allreduce-threshold\]](#def:mv2-shmem-coll-allreduce-threshold){reference-type="ref"
reference="def:mv2-shmem-coll-allreduce-threshold"}),
MV2_SHMEM_REDUCE_MSG
([\[def:mv2-shmem-coll-reduce-threshold\]](#def:mv2-shmem-coll-reduce-threshold){reference-type="ref"
reference="def:mv2-shmem-coll-reduce-threshold"}), the new 2-level
algorithms for medium sized messages and the default point-to-point
based algorithms for large messages. We have introduced two new run-time
variables MV2_ALLREDUCE_2LEVEL_MSG
([\[def:mv2_allreduce_2level_msg\]](#def:mv2_allreduce_2level_msg){reference-type="ref"
reference="def:mv2_allreduce_2level_msg"}) and MV2_REDUCE_2LEVEL_MSG
([\[def:mv2_reduce_2level_msg\]](#def:mv2_reduce_2level_msg){reference-type="ref"
reference="def:mv2_reduce_2level_msg"}) to determine when to fall back
to the default point-to-point based algorithms.

### Optimizations for MPI_Gather and MPI_Scatter

MVAPICH2 supports two new optimized algorithms for MPI_Gather and
MPI_Scatter operations -- the "Direct" and the multi-core aware
"2-level" algorithms. Both these algorithms perform significantly better
than the default binomial-tree pattern. The "Direct" algorithm is
however inherently not very scalable and can be used when the
communicator size is less than 1K processes. We switch over to the
2-level algorithms for larger system sizes. For MPI_Gather, we use
different algorithms depending of the system size. For small system
sizes (up to 386 cores), we use the "2-level" algorithm following by the
"Direct\" algorithm. For medium system sizes (up to 1k), we use
"Binomial" algorithm following by the "Direct" algorithm. It's possible
to set the switching point between algorithms using the run-time
parameter MV2_GATHER_SWITCH_PT
([\[def:mv2_gather_switch_pt\]](#def:mv2_gather_switch_pt){reference-type="ref"
reference="def:mv2_gather_switch_pt"}). For MPI_Scatter, when the system
size is lower than 512 cores, we use the "Binomial" algorithm for small
message sizes following by the "2-level" algorithm for medium message
sizes and the "Direct" algorithm for large message sizes. Users can
define the threshold for small and medium message sizes using the
run-time parameters MV2_SCATTER_SMALL_MSG
([\[def:mv2_scatter_small_msg\]](#def:mv2_scatter_small_msg){reference-type="ref"
reference="def:mv2_scatter_small_msg"}) and MV2_SCATTER_MEDIUM_MSG
([\[def:mv2_scatter_medium_msg\]](#def:mv2_scatter_medium_msg){reference-type="ref"
reference="def:mv2_scatter_medium_msg"}). Users can also choose to use
only one of these algorithms by toggling the run-time parameters
 [\[def:mv2_use_direct_gather\]](#def:mv2_use_direct_gather){reference-type="ref"
reference="def:mv2_use_direct_gather"} and
 [\[def:mv2_use_two_level_gather\]](#def:mv2_use_two_level_gather){reference-type="ref"
reference="def:mv2_use_two_level_gather"} for MPI_Gather and
 [\[def:mv2_use_direct_scatter\]](#def:mv2_use_direct_scatter){reference-type="ref"
reference="def:mv2_use_direct_scatter"} and
 [\[def:mv2_use_two_level_scatter\]](#def:mv2_use_two_level_scatter){reference-type="ref"
reference="def:mv2_use_two_level_scatter"} for MPI_Scatter.

## Process Placement on Multi-core platforms

Process placement has a significant impact on performance of
applications. Depending on your application communication patterns,
various process placements can result in performance gains. In
Section [\[sec:usage:mv2-cpu-mapping\]](#sec:usage:mv2-cpu-mapping){reference-type="ref"
reference="sec:usage:mv2-cpu-mapping"}, we have described the usage of
"bunch" and "scatter" placement modes provided by MVAPICH2. Using these
modes, one can control the placement of processes within a particular
node. Placement of processes across nodes can be controlled by adjusting
the order of MPI ranks. For example, the following command launches jobs
in block fashion.

The following command launches jobs in a cyclic fashion.

We have noted that the HPL (High-Performance Linpack) benchmark performs
better when using block distribution.

## HugePage Support

MVAPICH2 uses HugePages(2MB) by default for communication buffers if
they are configured on the system. The run-time variable,
MV2_USE_HUGEPAGES( [\[def:use-hugepage\]](#def:use-hugepage){reference-type="ref"
reference="def:use-hugepage"}) can be used to control the behavior of
this feature.

In order to use HugePages, Make sure HugePages are configured on all
nodes. The number of HugePages can be configured by setting
vm.nr_hugepages kernel parameter to a suitable value. For example, to
allocate a 1GB HugePage pool, execute(as root):\
\
\
or\
\
