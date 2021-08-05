# MVAPICH2 Parameters (OFA-IB-Nemesis Interface) 

## MV2_DEFAULT_MAX_SEND_WQE 

-   Class: Run time

-   Default: 64

This specifies the maximum number of send WQEs on each QP. Please note
that for Gen2 and Gen2-iWARP, the default value of this parameter will
be 16 if the number of processes is larger than 256 for better memory
scalability.

## MV2_DEFAULT_MAX_RECV_WQE 

-   Class: Run time

-   Default: 128

This specifies the maximum number of receive WQEs on each QP (maximum
number of receives that can be posted on a single QP).

## MV2_DEFAULT_MTU 

-   Class: Run time

-   Default: IBV_MTU_1024 for IB SDR cards and IBV_MTU_2048 for IB DDR
    and QDR cards.

The internal MTU size. For Gen2, this parameter should be a string
instead of an integer. Valid values are: `IBV_MTU_256`, `IBV_MTU_512`,
`IBV_MTU_1024`, `IBV_MTU_2048`, `IBV_MTU_4096`.

## MV2_DEFAULT_PKEY 

-   Class: Run Time

Select the partition to be used for the job.

## MV2_IBA_EAGER_THRESHOLD 

-   Class: Run time

-   Default: Architecture dependent (12KB for IA-32)

This specifies the switch point between eager and rendezvous protocol in
MVAPICH2. For better performance, the value of MV2_IBA_EAGER_THRESHOLD
should be set the same as MV2_VBUF_TOTAL_SIZE.

## MV2_IBA_HCA 

-   Class: Run time

-   Default: Unset

This specifies the HCA to be used for performing network operations.

## MV2_INITIAL_PREPOST_DEPTH 

-   Class: Run time

-   Default: 10

This defines the initial number of pre-posted receive buffers for each
connection. If communication happen for a particular connection, the
number of buffers will be increased to\
RDMA_PREPOST_DEPTH.

## MV2_MAX_INLINE_SIZE 

-   Class: Run time

-   Default: Network card dependent (128 for most networks including
    InfiniBand)

This defines the maximum inline size for data transfer. Please note that
the default value of this parameter will be 0 when the number of
processes is larger than 256 to improve memory usage scalability.

## MV2_NDREG_ENTRIES 

-   Class: Run time

-   Default: 1000

This defines the total number of buffers that can be stored in the
registration cache. It has no effect if MV2_USE_LAZY_MEM_UNREGISTER is
not set. A larger value will lead to less frequent lazy de-registration.

## MV2_NUM_RDMA_BUFFER 

-   Class: Run time

-   Default: Architecture dependent (32 for EM64T)

The number of RDMA buffers used for the RDMA fast path. This *fast path*
is used to reduce latency and overhead of small data and control
messages. This value will be ineffective if MV2_USE_RDMA_FAST_PATH is
not set.

## MV2_NUM_SA_QUERY_RETRIES 

-   Class: Run time

-   Default: 20

-   Applicable Interface(s): OFA-IB-CH3, OFA-iWARP-CH3

Number of times the MPI library will attempt to query the subnet to
obtain the path record information before giving up.

## MV2_PREPOST_DEPTH 

-   Class: Run time

-   Default: 64

This defines the number of buffers pre-posted for each connection to
handle send/receive operations.

## MV2_RNDV_PROTOCOL

-   Class: Run time

-   Default: RPUT

The value of this variable can be set to choose different Rendezvous
protocols. RPUT (default RDMA-Write) RGET (RDMA Read based), R3
(send/recv based).

## MV2_R3_THRESHOLD

-   Class: Run time

-   Default: 4096

The value of this variable controls what message sizes go over the R3
rendezvous protocol. Messages above this message size use
MV2_RNDV_PROTOCOL.

## MV2_R3_NOCACHE_THRESHOLD

-   Class: Run time

-   Default: 32768

The value of this variable controls what message sizes go over the R3
rendezvous protocol when the registration cache is disabled
(MV2_USE_LAZY_MEM_UNREGISTER=0). Messages above this message size use
MV2_RNDV_PROTOCOL.

## MV2_SRQ_LIMIT 

-   Class: Run Time

-   Default: 30

This is the low water-mark limit for the Shared Receive Queue. If the
number of available work entries on the SRQ drops below this limit, the
flow control will be activated.

## MV2_SRQ_SIZE 

-   Class: Run Time

-   Default: 512

This is the maximum number of work requests allowed on the Shared
Receive Queue.

## MV2_STRIPING_THRESHOLD 

-   Class: Run Time

-   Default: 8192

This parameter specifies the message size above which we begin the
stripe the message across multiple rails (if present).

## MV2_USE_BLOCKING

-   Class: Run time

-   Default: 0

Setting this parameter enables mvapich2 to use blocking mode progress.
MPI applications do not take up any CPU when they are waiting for
incoming messages.

## MV2_USE_LAZY_MEM_UNREGISTER

-   Class: Run time

-   Default: set

Setting this parameter enables mvapich2 to use memory registration
cache.

## MV2_USE_RDMA_FAST_PATH 

-   Class: Run time

-   Default: set

Setting this parameter enables MVAPICH2 to use adaptive rdma fast path
features for the Gen2 interface.

## MV2_USE_SRQ

-   Class: Run time

-   Default: set

Setting this parameter enables mvapich2 to use shared receive queue.

## MV2_VBUF_POOL_SIZE 

-   Class: Run time

-   Default: 512

The number of vbufs in the initial pool. This pool is shared among all
the connections.

## MV2_VBUF_SECONDARY_POOL_SIZE 

-   Class: Run time

-   Default: 128

The number of vbufs allocated each time when the global pool is running
out in the initial pool. This is also shared among all the connections.

## MV2_VBUF_TOTAL_SIZE 

-   Class: Run time

-   Default: Architecture dependent (6 KB for EM64T)

The size of each `vbuf`, the basic communication buffer of MVAPICH2. For
better performance, the value of MV2_IBA_EAGER_THRESHOLD should be set
the same as MV2_VBUF_TOTAL_SIZE.

## MV2_RUN_THROUGH_STABILIZATION 

-   Class: Run Time

-   Default: 0

This enables run through stabilization support to handle the process
failures. This is valid only with Hydra process manager with
--disable-auto-cleanup flag.
