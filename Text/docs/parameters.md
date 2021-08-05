# MVAPICH2 Parameters (CH3-Based Interfaces) 

## MV2_ALLREDUCE_2LEVEL_MSG 

-   Class: Run Time

-   Default: 256K Bytes

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This parameter can be used to determine the threshold for the 2-level
Allreduce algorithm. We now use the shared-memory-based algorithm for
messages smaller than the\
MV2_SHMEM_ALLREDUCE_MSG threshold
([1.57](#def:mv2-shmem-coll-allreduce-threshold){reference-type="ref"
reference="def:mv2-shmem-coll-allreduce-threshold"}), the 2-level
algorithm for medium sized messages up to the threshold defined by this
parameter. We use the default point-to-point algorithms messages larger
than this threshold.

## MV2_CKPT_AGGREGATION_BUFPOOL_SIZE 

-   Class: Run Time

-   Default: 8M

-   Applicable interface(s): OFA-IB-CH3

This parameter determines the size of the buffer pool reserved for use
in checkpoint aggregation. Note that this variable can be set with
suffixes such as 'K'/'k', 'M'/'m' or 'G'/'g' to denote Kilobyte,
Megabyte or Gigabyte respectively.

## MV2_CKPT_AGGREGATION_CHUNK_SIZE 

-   Class: Run Time

-   Default: 1M

-   Applicable interface(s): OFA-IB-CH3

The checkpoint data that has been coalesced into the buffer pool, is
written to the back-end file system, with the value of this parameter as
the chunk size. Note that this variable can be set with suffixes such as
'K'/'k', 'M'/'m' or 'G'/'g' to denote Kilobyte, Megabyte or Gigabyte
respectively.

## MV2_CKPT_FILE 

-   Class: Run Time

-   Default: /tmp/ckpt

-   Applicable interface(s): OFA-IB-CH3

This parameter specifies the path and the base file name for checkpoint
files of MPI processes. The checkpoint files will be named as
\$MV2_CKPT_FILE.$<$number of checkpoint$>$.$<$process rank$>$, for
example, /tmp/ckpt.1.0 is the checkpoint file for process 0's first
checkpoint. To checkpoint on network-based file systems, user just need
to specify the path to it, such as /mnt/pvfs2/my_ckpt_file.

## MV2_CKPT_INTERVAL 

-   Class: Run Time

-   Default: 0

-   Unit: minutes

-   Applicable interface(s): OFA-IB-CH3

This parameter can be used to enable automatic checkpointing. To let MPI
job console automatically take checkpoints, this value needs to be set
to the desired checkpointing interval. A zero will disable automatic
checkpointing. Using automatic checkpointing, the checkpoint file for
the MPI job console will be named as \$MV2_CKPT_FILE.$<$number of
checkpoint$>$.auto. Users need to use this file for restart.

## MV2_CKPT_MAX_SAVE_CKPTS 

-   Class: Run Time

-   Default: 0

-   Applicable interface(s): OFA-IB-CH3

This parameter is used to limit the number of checkpoints saved on file
system to save the file system space. When set to a positive value N,
only the last N checkpoints will be saved.

## MV2_CKPT_NO_SYNC 

-   Class: Run Time

-   Applicable interface(s): OFA-IB-CH3

When this parameter is set to any value, the checkpoints will not be
required to sync to disk. It can reduce the checkpointing delay in many
cases. But if users are using local file system, or any parallel file
system with local cache, to store the checkpoints, it is recommended not
to set this parameter because otherwise the checkpoint files will be
cached in local memory and will likely be lost upon failure.

## MV2_CKPT_USE_AGGREGATION 

-   Class: Run Time

-   Default: 1 (if configured with Checkpoint Aggregation support)

-   Applicable interface(s): OFA-IB-CH3

This parameter enables/disables Checkpoint aggregation scheme at run
time. It is set to '1'(enabled) by default, when the user enables
Checkpoint/Restart functionality at configure time, or when the user
explicitly configures MVAPICH2 with aggregation support. Please note
that, to use aggregation support, each node needs to be properly
configured with FUSE library (cf
section [\[para:mpi-cr-aggr\]](#para:mpi-cr-aggr){reference-type="ref"
reference="para:mpi-cr-aggr"}).

## MV2_DEBUG_FT_VERBOSE 

-   Class: Run Time

-   Type: Null or positive integer

-   Default: 0 (disabled)

This parameter enables/disables the debug output for Fault Tolerance
features (Checkpoint/Restart and Migration).

Note: All debug output is disabled when MVAPICH2 is configured with the
`–enable-fast=ndebug` option.

## MV2_CM_RECV_BUFFERS 

-   Class: Run Time

-   Default: 1024

-   Applicable interface(s): OFA-IB-CH3

This defines the number of buffers used by connection manager to
establish new connections. These buffers are quite small and are shared
for all connections, so this value may be increased to 8192 for large
clusters to avoid retries in case of packet drops.

## MV2_CM_SPIN_COUNT 

-   Class: Run Time

-   Default: 5000

-   Applicable interface(s): OFA-IB-CH3

This is the number of the connection manager polls for new control
messages from UD channel for each interrupt. This may be increased to
reduce the interrupt overhead when many incoming control messages from
UD channel at the same time.

## MV2_CM_TIMEOUT 

-   Class: Run Time

-   Default: 500

-   Unit: milliseconds

-   Applicable interface(s): OFA-IB-CH3

This is the timeout value associated with connection management messages
via UD channel. Decreasing this value may lead to faster retries but at
the cost of generating duplicate messages.

## MV2_CPU_MAPPING 

-   Class: Run Time

-   Default: NA

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3, PSM

This allows users to specify process to CPU (core) mapping. The detailed
usage of this parameter is described in
Section [\[usage:mv2_cpu_mapping\]](#usage:mv2_cpu_mapping){reference-type="ref"
reference="usage:mv2_cpu_mapping"}. This parameter will not take effect
if either *MV2_ENABLE_AFFINITY* or *MV2_USE_SHARED_MEM* run-time
parameters are set to 0, or if the library was configured with the
"--disable-hwloc" option. MV2_CPU_MAPPING is currently not supported on
Solaris.

## MV2_CPU_BINDING_POLICY 

-   Class: Run Time

-   Default: hybrid

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3, PSM

We have changed the default value of MV2_CPU_BINDING_POLICY to "hybrid"
along with MV2_HYBRID_BINDING_POLICY=bunch. It is same as setting
MV2_CPU_BINDING_POLICY to bunch. However, it also works well for the
systems with hyper-threading enabled or systems that have vendor
specific core mappings. This allows users to specify process to CPU
(core) mapping with the CPU binding policy. The detailed usage of this
parameter is described in
Section [\[usage:mv2_use_hwloc_cpu_binding\]](#usage:mv2_use_hwloc_cpu_binding){reference-type="ref"
reference="usage:mv2_use_hwloc_cpu_binding"}. This parameter will not
take effect: if\
*MV2_ENABLE_AFFINITY* or *MV2_USE_SHARED_MEM* run-time parameters are
set to 0; or\
*MV2_ENABLE_AFFINITY* is set to 1 and *MV2_CPU_MAPPING* is set, or if
the library was configured with the "--disable-hwloc" option. The value
of MV2_CPU_BINDING_POLICY can be "bunch", "scatter", or "hybrid". When
this parameter takes effect and its value isn't set, "bunch" will be
used as the default policy.

## MV2_HYBRID_BINDING_POLICY 

-   Class: Run Time

-   Default: Linear

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3, PSM

This allows users to specify binding policies for application thread in
MPI+Threads applications. The detailed usage of this parameter is
described in
Section [\[sec:advanced_thread_binding_policies\]](#sec:advanced_thread_binding_policies){reference-type="ref"
reference="sec:advanced_thread_binding_policies"}. This parameter will
not take effect: if\
*MV2_ENABLE_AFFINITY* or *MV2_USE_SHARED_MEM* run-time parameters are
set to 0; or\
*MV2_CPU_BINDING_POLICY* is set to "bunch\" or "scatter\", or\
*MV2_ENABLE_AFFINITY* is set to 1 and *MV2_CPU_MAPPING* is set, or if
the library was configured with the "--disable-hwloc" option. The value
of MV2_HYBRID_BINDING_POLICY can be "linear", "compact", "bunch",
"scatter", "spread", or "numa". When this parameter takes effect and its
value isn't set, "linear" will be used as the default policy.

## MV2_CPU_BINDING_LEVEL 

-   Class: Run Time

-   Default: Core

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This allows users to specify process to CPU (core) mapping at different
binding level. The detailed usage of this parameter is described in
Section [\[usage:mv2_use_hwloc_cpu_binding\]](#usage:mv2_use_hwloc_cpu_binding){reference-type="ref"
reference="usage:mv2_use_hwloc_cpu_binding"}. This parameter will not
take effect: if *MV2_ENABLE_AFFINITY* or *MV2_USE_SHARED_MEM* run-time
parameters are set to 0; or `MV2_ENABLE_AFFINITY` is set to 1 and
`MV2_CPU_MAPPING` is set, or if the library was configured with the
"--disable-hwloc" option. The value of MV2_CPU_BINDING_LEVEL can be
"core", "socket", or "numanode". When this parameter takes effect and
its value isn't set, "core" will be used as the default binding level.

## MV2_SHOW_HCA_BINDING 

-   Class: Run time

-   Default: 0 (disabled)

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

-   Possible values: 1 (Show only on node containing rank 0), 2 (Show on
    all nodes)

If set to 1, it shows the HCA binding of all processes on node where
rank 0 exists. If set to 2, it shows the HCA binding of all processes on
all nodes.

## MV2_DEFAULT_MAX_SEND_WQE 

-   Class: Run time

-   Default: 64

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This specifies the maximum number of send WQEs on each QP. Please note
that for OFA-IB-CH3 and OFA-iWARP-CH3, the default value of this
parameter will be 16 if the number of processes is larger than 256 for
better memory scalability.

## MV2_DEFAULT_MAX_RECV_WQE 

-   Class: Run time

-   Default: 128

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This specifies the maximum number of receive WQEs on each QP (maximum
number of receives that can be posted on a single QP).

## MV2_DEFAULT_MTU 

-   Class: Run time

-   Default: OFA-IB-CH3: IBV_MTU_1024 for IB SDR cards and IBV_MTU_2048
    for IB DDR and QDR cards.

-   Applicable interface(s): OFA-IB-CH3

The internal MTU size. For OFA-IB-CH3, this parameter should be a string
instead of an integer. Valid values are: `IBV_MTU_256`, `IBV_MTU_512`,
`IBV_MTU_1024`, `IBV_MTU_2048`, `IBV_MTU_4096`.

## MV2_DEFAULT_PKEY 

-   Class: Run Time

-   Applicable Interface(s): OFA-IB-CH3

Select the partition to be used for the job.

## MV2_ENABLE_AFFINITY 

-   Class: Run time

-   Default: 1

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

Enable CPU affinity by setting MV2_ENABLE_AFFINITY to 1 or disable it by
setting\
MV2_ENABLE_AFFINITY to 0. MV2_ENABLE_AFFINITY is currently not supported
on Solaris. CPU affinity is also not supported if MV2_USE_SHARED_MEM is
set to 0.

## MV2_GET_FALLBACK_THRESHOLD

-   Class: Run time

-   This threshold value needs to be set in bytes.

-   This option is effective if we define ONE_SIDED flag.

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This defines the threshold beyond which the MPI_Get implementation is
based on direct one sided RDMA operations.

## MV2_IBA_EAGER_THRESHOLD 

-   Class: Run time

-   Default: Host Channel Adapter (HCA) dependent (12 KB for ConnectX
    HCA's)

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This specifies the switch point between eager and rendezvous protocol in
MVAPICH2. For better performance, the value of MV2_IBA_EAGER_THRESHOLD
should be set the same as MV2_VBUF_TOTAL_SIZE.

## MV2_IBA_HCA 

-   Class: Run time

-   Default: Unset

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This specifies the HCA's to be used for performing network operations.

## MV2_INITIAL_PREPOST_DEPTH 

-   Class: Run time

-   Default: 10

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This defines the initial number of pre-posted receive buffers for each
connection. If communication happen for a particular connection, the
number of buffers will be increased to\
RDMA_PREPOST_DEPTH.

## MV2_IWARP_MULTIPLE_CQ_THRESHOLD 

-   Class: Run time

-   Default: 32

-   Applicable interface(s): OFA-iWARP-CH3

This defines the process size beyond which we use multiple completion
queues for iWARP interface.

## MV2_KNOMIAL_INTRA_NODE_FACTOR 

-   Class: Run time

-   Default: 4

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This defines the degree of the knomial operation during the intra-node
knomial broadcast phase.

## MV2_KNOMIAL_INTER_NODE_FACTOR 

-   Class: Run time

-   Default: 4

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This defines the degree of the knomial operation during the inter-node
knomial broadcast phase.

## MV2_MAX_INLINE_SIZE 

-   Class: Run time

-   Default: Network card dependent (128 for most networks including
    InfiniBand)

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This defines the maximum inline size for data transfer. Please note that
the default value of this parameter will be 0 when the number of
processes is larger than 256 to improve memory usage scalability.

## MV2_MAX_NUM_WIN 

-   Class: Run time

-   Default: 64

-   Applicable interface(s): OFA-IB-CH3

Maximum number of RMA windows that can be created and active
concurrently. Typically this value is sufficient for most applications.
Increase this value to the number of windows your application uses

## MV2_NDREG_ENTRIES 

-   Class: Run time

-   Default: Represented by RDMA_NDREG_ENTRIES (value is 1100). Depends
    on the number of processes.

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This defines the total number of buffers that can be stored in the
registration cache. It has no effect if MV2_USE_LAZY_MEM_UNREGISTER is
not set. A larger value will lead to less frequent lazy de-registration.

## MV2_NUM_HCAS 

-   Class: Run time

-   Default: 1

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This parameter indicates number of InfiniBand adapters to be used for
communication on an end node.

## MV2_NUM_PORTS 

-   Class: Run time

-   Default: 1

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This parameter indicates number of ports per InfiniBand adapter to be
used for communication per adapter on an end node.

## MV2_DEFAULT_PORT 

-   Class: Run time

-   Default: none

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This parameter is to select the specific HCA port on a active multi port
InfiniBand adapter

## MV2_NUM_SA_QUERY_RETRIES 

-   Class: Run time

-   Default: 20

-   Applicable Interface(s): OFA-IB-CH3, OFA-iWARP-CH3

Number of times the MPI library will attempt to query the subnet to
obtain the path record information before giving up.

## MV2_NUM_QP_PER_PORT 

-   Class: Run time

-   Default: 1

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This parameter indicates number of queue pairs per port to be used for
communication on an end node. This is useful in the presence of multiple
send/recv engines available per port for data transfer.

## MV2_RAIL_SHARING_POLICY 

-   Class: Run time

-   Default: Rail Binding in round-robin

-   Value Domain: USE_FIRST, ROUND_ROBIN, FIXED_MAPPING

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This specifies the policy that will be used to assign HCAs to each of
the processes. In the previous versions of MVAPICH2 it was known as
MV2_SM_SCHEDULING.

## MV2_RAIL_SHARING_LARGE_MSG_THRESHOLD 

-   Class: Run time

-   Default: 16K

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This specifies the threshold for the message size beyond which striping
will take place. In the previous versions of MVAPICH2 it was known as
MV2_STRIPING_THRESHOLD

## MV2_PROCESS_TO_RAIL_MAPPING 

-   Class: Run time

-   Default: NONE

-   Value Domain: BUNCH, SCATTER, $<$CUSTOM LIST$>$

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

When MV2_RAIL_SHARING_POLICY is set to the value "FIXED_MAPPING" this
variable decides the manner in which the HCAs will be mapped to the
rails. The $<$CUSTOM LIST$>$ is colon(:) separated list with the HCA
ranks specified. e.g. 0:1:1:0. This list must map equally to the number
of local processes on the nodes failing which, the default policy will
be used. Similarly the number of processes on each node must be the
same. The detailed usage of this parameter is described in
Section [\[subsec:mpi-mr\]](#subsec:mpi-mr){reference-type="ref"
reference="subsec:mpi-mr"}.

## MV2_RDMA_FAST_PATH_BUF_SIZE 

-   Class: Run time

-   Default: Architecture dependent

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

The size of the buffer used in RDMA fast path communication. This value
will be ineffective if\
MV2_USE_RDMA_FAST_PATH is not set

## MV2_NUM_RDMA_BUFFER 

-   Class: Run time

-   Default: Architecture dependent (32 for EM64T)

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

The number of RDMA buffers used for the RDMA fast path. This *fast path*
is used to reduce latency and overhead of small data and control
messages. This value will be ineffective if MV2_USE_RDMA_FAST_PATH is
not set.

## MV2_ON_DEMAND_THRESHOLD 

-   Class: Run Time

-   Default: 64 (OFA-IB-CH3), 16 (OFA-iWARP-CH3)

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This defines threshold for enabling on-demand connection management
scheme. When the size of the job is larger than the threshold value,
on-demand connection management will be used.

## MV2_HOMOGENEOUS_CLUSTER 

-   Class: Run Time

-   Default: 0

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

Set this parameter to 1 on homogeneous clusters to optimize the job
start-up

## MV2_PREPOST_DEPTH 

-   Class: Run time

-   Default: 64

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This defines the number of buffers pre-posted for each connection to
handle send/receive operations.

## MV2_PSM_DEBUG 

-   Class: Run time (Debug)

-   Default: 0

-   Applicable interface: PSM

This parameter enables the dumping of run-time debug counters from the
MVAPICH2-PSM progress engine. Counters are dumped every
PSM_DUMP_FREQUENCY seconds.

## MV2_PSM_DUMP_FREQUENCY 

-   Class: Run time (Debug)

-   Default: 10 seconds

-   Applicable interface: PSM

This parameters sets the frequency for dumping MVAPICH2-PSM debug
counters. Value takes effect only in PSM_DEBUG is enabled.

## MV2_PUT_FALLBACK_THRESHOLD

-   Class: Run time

-   This threshold value needs to be set in bytes.

-   This option is effective if we define ONE_SIDED flag.

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This defines the threshold beyond which the MPI_Put implementation is
based on direct one sided RDMA operations.

## MV2_RAIL_SHARING_LARGE_MSG_THRESHOLD 

-   Class: Run Time

-   Default: 16 KB

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This parameter specifies the message size above which we begin the
stripe the message across multiple rails (if present).

## MV2_RDMA_CM_ARP_TIMEOUT 

-   Class: Run Time

-   Default: 2000 ms

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3, OFA-RoCE-CH3

This parameter specifies the ARP timeout to be used by RDMA CM module.

## MV2_RDMA_CM_MAX_PORT 

-   Class: Run Time

-   Default: Unset

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3, OFA-RoCE-CH3

This parameter specifies the upper limit of the port range to be used by
the RDMA CM module when choosing the port on which it listens for
connections.

## MV2_RDMA_CM_MIN_PORT 

-   Class: Run Time

-   Default: Unset

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3, OFA-RoCE-CH3

This parameter specifies the lower limit of the port range to be used by
the RDMA CM module when choosing the port on which it listens for
connections.

## MV2_REDUCE_2LEVEL_MSG 

-   Class: Run Time

-   Default: 32K Bytes.

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This parameter can be used to determine the threshold for the 2-level
reduce algorithm. We now use the shared-memory-based algorithm for
messages smaller than the MV2_SHMEM_REDUCE_MSG
([1.63](#def:mv2-shmem-coll-reduce-threshold){reference-type="ref"
reference="def:mv2-shmem-coll-reduce-threshold"}), the 2-level algorithm
for medium sized messages up to the threshold defined by this parameter.
We use the default point-to-point algorithms messages larger than this
threshold.

## MV2_RNDV_PROTOCOL 

-   Class: Run time

-   Default: RPUT

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

The value of this variable can be set to choose different Rendezvous
protocols. RPUT (default RDMA-Write) RGET (RDMA Read based), R3
(send/recv based).

## MV2_R3_THRESHOLD 

-   Class: Run time

-   Default: MV2_IBA_EAGER_THRESHOLD

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

The value of this variable controls what message sizes go over the R3
rendezvous protocol. Messages above this message size use
MV2_RNDV_PROTOCOL.

## MV2_R3_NOCACHE_THRESHOLD

-   Class: Run time

-   Default: 32768

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

The value of this variable controls what message sizes go over the R3
rendezvous protocol when the registration cache is disabled
(MV2_USE_LAZY_MEM_UNREGISTER=0). Messages above this message size use
MV2_RNDV_PROTOCOL.

## MV2_SHMEM_ALLREDUCE_MSG 

-   Class: Run Time

-   Default: 1 $\ll$ 15

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

The SHMEM AllReduce is used for messages less than this threshold.

## MV2_SHMEM_BCAST_LEADERS 

-   Class: Run time

-   Default: 4096

The number of leader processes that will take part in the SHMEM
broadcast operation. Must be greater than the number of nodes in the
job.

## MV2_SHMEM_BCAST_MSG 

-   Class: Run Time

-   Default: 1 $\ll$ 20

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

The SHMEM bcast is used for messages less than this threshold.

## MV2_SHMEM_COLL_MAX_MSG_SIZE 

-   Class: Run Time

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This parameter can be used to select the max buffer size of message for
shared memory collectives.

## MV2_SHMEM_COLL_NUM_COMM 

-   Class: Run Time

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This parameter can be used to select the number of communicators using
shared memory collectives.

## MV2_SHMEM_DIR 

-   Class: Run Time

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

-   Default: /dev/shm for Linux and /tmp for Solaris

This parameter can be used to specify the path to the shared memory
files for intra-node communication.

## MV2_SHMEM_REDUCE_MSG 

-   Class: Run Time

-   Default: 1 $\ll$ 13

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

The SHMEM reduce is used for messages less than this threshold.

## MV2_SM_SCHEDULING 

-   Class: Run Time

-   Default: USE_FIRST (Options: ROUND_ROBIN)

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

## MV2_SMP_USE_LIMIC2 

-   Class: Run Time

-   Default: On if configured with --with-limic2

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This parameter enables/disables LiMIC2 at run time. It does not take
effect if MVAPICH2 is not configured with --with-limic2.

## MV2_SMP_USE_CMA 

-   Class: Run Time

-   Default: On unless configured with --without-cma

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This parameter enables/disables CMA based intra-node communication at
run time. It does not take effect if MVAPICH2 is configured with
--without-cma. When --with-limic2 is included in the configure flags,
LiMIC2 is used in preference over CMA. Please set MV2_SMP_USE_LIMIC2 to
0 in order to choose CMA if MVAPICH2 is configured with --with-limic2.

## MV2_SRQ_LIMIT 

-   Class: Run Time

-   Default: 30

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This is the low water-mark limit for the Shared Receive Queue. If the
number of available work entries on the SRQ drops below this limit, the
flow control will be activated.

## MV2_SRQ_MAX_SIZE 

-   Class: Run Time

-   Default: 4096

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This is the maximum number of work requests allowed on the Shared
Receive Queue. Upon receiving a SRQ limit event, the current value of
MV2_SRQ_SIZE will be doubled or moved to the maximum of
MV2_SRQ_MAX_SIZE, whichever is smaller

## MV2_SRQ_SIZE 

-   Class: Run Time

-   Default: 256

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This is the initial number of work requests posted to the Shared Receive
Queue.

## MV2_STRIPING_THRESHOLD 

-   Class: Run Time

-   Default: 8192

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This parameter specifies the message size above which we begin the
stripe the message across multiple rails (if present).

## MV2_SUPPORT_DPM 

-   Class: Run time

-   Default: 0 (disabled)

-   Applicable interface: OFA-IB-CH3

This option enables the dynamic process management interface and
on-demand connection management.

## MV2_USE_APM 

-   Class: Run Time

-   Applicable interface(s): OFA-IB-CH3

This parameter is used for recovery from network faults using Automatic
Path Migration. This functionality is beneficial in the presence of
multiple paths in the network, which can be enabled by using lmc
mechanism.

## MV2_USE_APM_TEST 

-   Class: Run Time

-   Applicable interface(s): OFA-IB-CH3

This parameter is used for testing the Automatic Path Migration
functionality. It periodically moves the alternate path as the primary
path of communication and re-loads another alternate path.

## MV2_USE_BLOCKING

-   Class: Run time

-   Default: 0

-   Applicable interface(s): OFA-IB-CH3

Setting this parameter enables MVAPICH2 to use blocking mode progress.
MPI applications do not take up any CPU when they are waiting for
incoming messages.

## MV2_USE_COALESCE

-   Class: Run time

-   Default: unset

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

Setting this parameter enables message coalescing to increase small
message throughput

## MV2_USE_DIRECT_GATHER 

-   Class: Run time

-   Default: set

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

Use the "Direct" algorithm for the MPI_Gather operation. If this
parameter is set to 0 at run-time, the "Direct" algorithm will not be
invoked.

## MV2_USE_DIRECT_SCATTER 

-   Class: Run time

-   Default: set

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

Use the "Direct" algorithm for the MPI_Scatter operation. If this
parameter is set to 0 at run-time, the "Direct" algorithm will not be
invoked.

## MV2_USE_HSAM 

-   Class: Run Time

-   Applicable interface(s): OFA-IB-CH3

This parameter is used for utilizing hot-spot avoidance with InfiniBand
clusters. To leverage this functionality, the subnet should be
configured with lmc greater than zero. Please refer to
section [\[def:mv2-hsam\]](#def:mv2-hsam){reference-type="ref"
reference="def:mv2-hsam"} for detailed information.

## MV2_USE_IWARP_MODE 

-   Class: Run Time

-   Default: unset

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This parameter enables the library to run in iWARP mode.

## MV2_USE_LAZY_MEM_UNREGISTER 

-   Class: Run time

-   Default: set

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

Setting this parameter enables MVAPICH2 to use memory registration
cache.

## MV2_USE_RoCE 

-   Class: Run Time

-   Default: Un Set

-   Applicable interface(s): OFA-IB-CH3

This parameter enables the use of RDMA over Ethernet for MPI
communication. The underlying HCA and network must support this feature.

## MV2_DEFAULT_GID_INDEX 

-   Class: Run Time

-   Default: 0

-   Applicable interface(s): OFA-IB-CH3

In RoCE mode, this parameter allows to choose non-default GID index in
loss-less ethernet setup using VLANs

## MV2_USE_RDMA_CM 

-   Class: Run Time

-   Default: Network Dependent (set for OFA-iWARP-CH3 and unset for
    OFA-IB-CH3/OFA-RoCE-CH3)

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3, OFA-RoCE-CH3

This parameter enables the use of RDMA CM for establishing the
connections.

## MV2_RDMA_CM_MULTI_SUBNET_SUPPORT 

-   Class: Run Time

-   Default: Unset

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This parameter allows MPI jobs to be run across multiple subnets
interconnected by InfiniBand routers. Note that this requires RDMA_CM
support to be enabled at configure time and runtime. Note that, RDMA_CM
support is enabled by default at configure time. At runtime, the
MV2_USE_RDMA_CM environment variable described in
Section [1.83](#def:mv2-use-rdma-cm){reference-type="ref"
reference="def:mv2-use-rdma-cm"} must be set to 1.

## MV2_RDMA_CM_CONF_FILE_PATH 

-   Class: Run Time

-   Default: Network Dependent (set for OFA-iWARP-CH3 and unset for
    OFA-IB-CH3/OFA-RoCE-CH3)

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3, OFA-RoCE-CH3

This parameter is to specify the path to mv2.conf file. If this is not
given, then it searches in the default location /etc/mv2.conf

## MV2_USE_RDMA_FAST_PATH 

-   Class: Run time

-   Default: set

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

Setting this parameter enables MVAPICH2 to use adaptive RDMA fast path
features for OFA-IB-CH3 interface.

## MV2_USE_RDMA_ONE_SIDED

-   Class: Run time

-   Default: set

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

Setting this parameter allows MVAPICH2 to use optimized one sided
implementation based RDMA operations.

## MV2_USE_RING_STARTUP

-   Class: Run time

-   Default: set

-   Applicable interface(s): OFA-IB-CH3

Setting this parameter enables MVAPICH2 to use ring based start up.

## MV2_USE_SHARED_MEM 

-   Class: Run time

-   Default: set

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

Use shared memory for intra-node communication.

## MV2_USE_SHMEM_ALLREDUCE 

-   Class: Run Time

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This parameter can be used to turn off shared memory based MPI_Allreduce
for OFA-IB-CH3 over IBA by setting this to 0.

## MV2_USE_SHMEM_BARRIER 

-   Class: Run Time

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This parameter can be used to turn off shared memory based MPI_Barrier
for OFA-IB-CH3 over IBA by setting this to 0.

## MV2_USE_SHMEM_BCAST 

-   Class: Run Time

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This parameter can be used to turn off shared memory based MPI_Bcast for
OFA-IB-CH3 over IBA by setting this to 0.

## MV2_USE_SHMEM_COLL 

-   Class: Run time

-   Default: set

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

Use shared memory for collective communication. Set this to 0 for
disabling shared memory collectives.

## MV2_USE_SHMEM_REDUCE 

-   Class: Run Time

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This parameter can be used to turn off shared memory based MPI_Reduce
for OFA-IB-CH3 over IBA by setting this to 0.

## MV2_USE_SRQ

-   Class: Run time

-   Default: set

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

Setting this parameter enables MVAPICH2 to use shared receive queue.

## MV2_GATHER_SWITCH_PT 

-   Class: Run time

-   Default: set

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

We use different algorithms depending on the system size. For small
system sizes (up to 386 cores), we use the "2-level" algorithm following
by the "Direct" algorithm. For medium system sizes (up to 1k), we use
"Binomial" algorithm following by the "Direct" algorithm. Users can set
the switching point between algorithms using the run-time parameter
MV2_GATHER_SWITCH_PT.

## MV2_SCATTER_SMALL_MSG 

-   Class: Run time

-   Default: set

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

When the system size is lower than 512 cores, we use the "Binomial"
algorithm for small message sizes. MV2_SCATTER_SMALL_MSG allows the
users to set the threshold for small messages.

## MV2_SCATTER_MEDIUM_MSG 

-   Class: Run time

-   Default: set

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

When the system size is lower than 512 cores, we use the "2-level"
algorithm for medium message sizes. MV2_SCATTER_MEDIUM_MSG allows the
users to set the threshold for medium messages.

## MV2_USE_TWO_LEVEL_GATHER 

-   Class: Run time

-   Default: set

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

Use the two-level multi-core-aware algorithm for the MPI_Gather
operation. If this parameter is set to 0 at run-time, the two-level
algorithm will not be invoked.

## MV2_USE_TWO_LEVEL_SCATTER 

-   Class: Run time

-   Default: set

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

Use the two-level multi-core-aware algorithm for the MPI_Scatter
operation. If this parameter is set to 0 at run-time, the two-level
algorithm will not be invoked.

## MV2_USE_XRC 

-   Class: Run time

-   Default: 0

-   Applicable Interface(s): OFA-IB-CH3

Use the XRC InfiniBand transport available since Mellanox ConnectX
adapters. This features requires OFED version later than 1.3. It also
automatically enables SRQ and ON-DEMAND connection management. Note that
the MVAPICH2 library needs to have been configured with --enable-xrc=yes
to use this feature.

## MV2_VBUF_POOL_SIZE 

-   Class: Run time

-   Default: 512

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

The number of vbufs in the initial pool. This pool is shared among all
the connections.

## MV2_VBUF_SECONDARY_POOL_SIZE 

-   Class: Run time

-   Default: 256

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

The number of vbufs allocated each time when the global pool is running
out in the initial pool. This is also shared among all the connections.

## MV2_VBUF_TOTAL_SIZE 

-   Class: Run time

-   Default: Host Channel Adapter (HCA) dependent (12 KB for ConnectX
    HCA's)

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

The size of each `vbuf`, the basic communication buffer of MVAPICH2. For
better performance, the value of MV2_IBA_EAGER_THRESHOLD should be set
the same as MV2_VBUF_TOTAL_SIZE.

## MV2_SMP_EAGERSIZE 

-   Class: Run time

-   Default: Architecture dependent

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This parameter defines the switch point from Eager protocol to
Rendezvous protocol for intra-node communication. Note that this
variable can be set with suffixes such as 'K'/'k', 'M'/'m' or 'G'/'g' to
denote Kilobyte, Megabyte or Gigabyte respectively.

## MV2_SMP_QUEUE_LENGTH 

-   Class: Run time

-   Default: Architecture dependent

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This parameter defines the size of shared buffer between every two
processes on the same node for transferring messages smaller than or
equal to MV2_SMP_EAGERSIZE. Note that this variable can be set with
suffixes such as 'K'/'k', 'M'/'m' or 'G'/'g' to denote Kilobyte,
Megabyte or Gigabyte respectively.

## MV2_SMP_NUM_SEND_BUFFER 

-   Class: Run time

-   Default: Architecture dependent

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This parameter defines the number of internal send buffers for sending
intra-node messages larger than MV2_SMP_EAGERSIZE.

## MV2_SMP_SEND_BUF_SIZE 

-   Class: Run time

-   Default: Architecture dependent

-   Applicable interface(s): OFA-IB-CH3 and OFA-iWARP-CH3

This parameter defines the packet size when sending intra-node messages
larger than\
MV2_SMP_EAGERSIZE.

## MV2_USE_HUGEPAGES 

-   Class: Run time

-   Default: 1

-   Applicable interface(s): OFA-IB-CH3

Set this to 0, to not use any HugePages.

## MV2_HYBRID_ENABLE_THRESHOLD 

-   Class: Run time

-   Default: 512

-   Applicable interface(s): OFA-IB-CH3

This defines the threshold for enabling Hybrid communication using UD
and RC/XRC. When the size of the job is greater than or equal to the
threshold value, Hybrid mode will be enabled. Otherwise, it uses default
RC/XRC connections for communication.

## MV2_HYBRID_MAX_RC_CONN 

-   Class: Run time

-   Default: 64

-   Applicable interface(s): OFA-IB-CH3

Maximum number of RC or XRC connections created per process. This limits
the amount of connection memory and prevents HCA QP cache thrashing.

## MV2_UD_PROGRESS_TIMEOUT 

-   Class: Run time

-   Default: System size dependent.

-   Applicable interface(s): OFA-IB-CH3

Time (usec) until ACK status is checked (and ACKs are sent if needed).
To avoid unnecessary retries, set this value less than
MV2_UD_RETRY_TIMEOUT. It is recommended to set this to 1/10 of\
MV2_UD_RETRY_TIMEOUT.

## MV2_UD_RETRY_TIMEOUT 

-   Class: Run time

-   Default: System size dependent.

-   Applicable interface(s): OFA-IB-CH3

Time (usec) after which an unacknowledged message will be retried

## MV2_UD_RETRY_COUNT 

-   Class: Run time

-   Default: System size dependent.

-   Applicable interface(s): OFA-IB-CH3

Number of retries of a message before the job is aborted. This is needed
in case of HCA fails.

## MV2_USE_UD_HYBRID 

-   Class: Run time

-   Default: 1

-   Applicable interface(s): OFA-IB-CH3

Set this to Zero, to disable UD transport in hybrid configuration mode.

## MV2_USE_ONLY_UD 

-   Class: Run time

-   Default: 0

-   Applicable interface(s): OFA-IB-CH3

Set this to 1, to enable only UD transport in hybrid configuration mode.
It will not use any RC/XRC connections in this mode.

## MV2_USE_UD_ZCOPY 

-   Class: Run time

-   Default: 1

-   Applicable interface(s): OFA-IB-CH3

Whether or not to use the zero-copy transfer mechanism to transfer large
messages on UD transport.

## MV2_USE_LIMIC_GATHER 

-   Class: Run time

-   Default: 0

-   Applicable interface(s): OFA-IB-CH3, PSM

If this flag is set to 1, we will use intra-node Zero-Copy MPI_Gather
designs, when the library has been configured to use LiMIC2.

## MV2_USE_MCAST 

-   Class: Run time

-   Default: 0

-   Applicable interface(s): OFA-IB-CH3

Set this to 1, to enable hardware multicast support in collective
communication

## MV2_MCAST_NUM_NODES_THRESHOLD 

-   Class: Run time

-   Default: 8

-   Applicable interface(s): OFA-IB-CH3

This defines the threshold for enabling multicast support in collective
communication. When MV2_USE_MCAST is set to 1 and the number of nodes in
the job is greater than or equal to the threshold value, it uses
multicast support in collective communication

## MV2_USE_CUDA 

-   Class: Run time

-   Default: 0

-   Applicable interface(s): OFA-IB-CH3

set this to One. to enable support for communication with GPU device
buffers.

## MV2_CUDA_BLOCK_SIZE 

-   Class: Run time

-   Default: 262144

-   Applicable interface(s): OFA-IB-CH3

The chunk size used in large message transfer from device memory to host
memory. The other suggested values for this parameter are 131072 and
524288.

## MV2_CUDA_KERNEL_VECTOR_TIDBLK_SIZE 

-   Class: Run time

-   Default: 1024

-   Applicable interface(s): OFA-IB-CH3

This controls the number of CUDA threads per block in pack/unpack
kernels for MPI vector datatype in communication involving GPU device
buffers.

## MV2_CUDA_KERNEL_VECTOR_YSIZE 

-   Class: Run time

-   Default: tuned based on dimensions of the vector

-   Applicable interface(s): OFA-IB-CH3

This controls the y-dimension of a thread block in pack/unpack kernels
for MPI vector datatype in communication involving GPU device buffers.
It controls the number of threads operating on each block of data in a
vector.

## MV2_CUDA_NONBLOCKING_STREAMS 

-   Class: Run time

-   Default: 1 (Enabled)

-   Applicable interface(s): OFA-IB-CH3

This controls the use of non-blocking streams for asynchronous CUDA
memory copies in communication involving GPU memory.

## MV2_CUDA_IPC 

-   Class: Run time

-   Default: 1

-   Applicable interface(s): OFA-IB-CH3

This enables intra-node GPU-GPU communication using IPC feature
available from CUDA 4.1

## MV2_CUDA_SMP_IPC 

-   Class: Run time

-   Default: 0

-   Applicable interface(s): OFA-IB-CH3

This enables an optimization for short message GPU device-to-device
communication using IPC feature available from CUDA 4.1

## MV2_ENABLE_SHARP 

-   Class: Run time

-   Default: 0

-   Applicable interface(s): OFA-IB-CH3

Set this to 1, to enable hardware SHArP support in collective
communication

## MV2_SHARP_HCA_NAME 

-   Class: Run time

-   Default: unset

-   Applicable interface(s): OFA-IB-CH3

By default, this is set by the MVAPICH2 library. However, you can
explicitly set the HCA name which is realized by the SHArP library.

## MV2_SHARP_PORT 

-   Class: Run time

-   Default: 1

-   Applicable interface(s): OFA-IB-CH3

By default, this is set by the MVAPICH2 library. However, you can
explicitly set the HCA port which is realized by the SHArP library.

## MV2_ENABLE_SOCKET_AWARE_COLLECTIVES 

-   Class : Run time

-   Default : Enabled (1)

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This parameter enables/disables support for socket-aware collective
communication. The parameter MV2_USE_SHMEM_COLL must be set to 1 for
this to work.

## MV2_USE_TOPO_AWARE_ALLREDUCE 

-   Class : Run time

-   Default : Enabled (1)

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This parameter determines whether a topology-aware algorithm should be
used or not for allreduce collective operations. It takes effect only if
MV2_ENABLE_TOPO_AWARE_COLLECTIVES is set to 1.

## MV2_USE_TOPO_AWARE_BARRIER 

-   Class : Run time

-   Default : Enabled (1)

-   Applicable interface(s): OFA-IB-CH3, OFA-iWARP-CH3

This parameter determines whether a topology-aware algorithm should be
used or not for barrier collective operations. It takes effect only if
MV2_ENABLE_TOPO_AWARE_COLLECTIVES is set to 1.

## MV2_USE_RDMA_CM_MCAST 

-   Class : Run time

-   Default : Enabled (1)

-   Applicable interface(s): OFA-IB-CH3

This parameter enables support for RDMA_CM based multicast group setup.
Requires the parameter MV2_USE_MCAST to be set to 1.
