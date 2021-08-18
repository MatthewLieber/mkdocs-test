# OSU Benchmarks 

If you have arrived at this point, you have successfully installed
MVAPICH2. Congratulations!! The OSU benchmarks should already be built
and installed along with MVAPICH2. Look for them under
\$prefix/libexec/mvapich2. Sample performance numbers for these
benchmarks on representative platforms with InfiniBand, iWARP and RoCE
adapters are also included on our projects' web page. You are welcome to
compare your performance numbers with our numbers. If you see any big
discrepancy, please let us know by sending an email to
<mvapich-discuss@cse.ohio-state.edu>.

The benchmarks provided are:


        MPI-1, MPI-2 and MPI-3   
                ------------------------ -----------------------------------------------------------------------------
        osu_bibw                 Bidirectional Bandwidth Test
        osu_bw                   Bandwidth Test
        osu_latency              Latency Test
        osu_latency_mt           Multi-threaded Latency Test
        osu_mbw_mr               Multiple Bandwidth / Message Rate Test
        osu_multi_lat            Multi-pair Latency Test
        osu_allgather            MPI_Allgather Latency Test
        osu_allgatherv           MPI_Allgatherv Latency Test
        osu_allreduce            MPI_Allreduce Latency Test
        osu_alltoall             MPI_Alltoall Latency Test
        osu_alltoallv            MPI_Alltoallv Latency Test
        osu_barrier              MPI_Barrier Latency Test
        osu_bcast                MPI_Bcast Latency Test
        osu_gather               MPI_Gather Latency Test
        osu_gatherv              MPI_Gatherv Latency Test
        osu_reduce               MPI_Reduce Latency Test
        osu_reduce_scatter       MPI_Reduce_scatter Latency Test
        osu_scatter              MPI_Scatter Latency Test
        osu_scatterv             MPI_Scatterv Latency Test
                                
        MPI-2 and MPI-3 only     
        osu_acc_latency          Accumulate Latency Test with Active/Passive Synchronization
        osu_get_bw               One-Sided Get Bandwidth Test with Active/Passive Synchronization
        osu_get_latency          One-Sided Get Latency Test with Active/Passive Synchronization
        osu_latency_mt           Multi-threaded Latency Test
        osu_put_bibw             One-Sided Put Bidirectional Test with Active Synchronization
        osu_put_bw               One-Sided Put Bandwidth Test with Active/Passive Synchronization
        osu_put_latency          One-Sided Put Latency Test with Active/Passive Synchronization
                                
        MPI-3 only               
        osu_cas_latency          One-Sided Compare_and_swap Latency Test with Active/Passive Synchronization
        osu_fop_latency          One-Sided Fetch_and_op Latency Test with Active/Passive Synchronization
        osu_get_acc_latency      One-Sided Get_accumulate Latency Test with Active/Passive Synchronization
        osu_iallgather           MPI_Iallgather Latency Test
        osu_iallgatherv          MPI_Iallgatherv Latency Test
        osu_iallreduce           MPI_Iallreduce Latency Test
        osu_ialltoall            MPI_Ialltoall Latency Test
        osu_ialltoallv           MPI_Ialltoallv Latency Test
        osu_ialltoallw           MPI_Ialltoallw Latency Test
        osu_ibarrier             MPI_Ibarrier Latency Test
        osu_ibcast               MPI_Ibcast Latency Test
        osu_igather              MPI_Igather Latency Test
        osu_igatherv             MPI_Igatherv Latency Test
        osu_ireduce              MPI_Ireduce Latency Test
        osu_iscatter             MPI_Iscatter Latency Test
        osu_iscatterv            MPI_Iscatterv Latency Test
                            


More information about the benchmarks can be found at
<http://mvapich.cse.ohio-state.edu/benchmarks/>. You can also check this
link for updates to the benchmarks.

## Download and Build Stand-alone OSU Benchmarks Package

The OSU Benchmarks can also be downloaded as a separate package from
[here](http://mvapich.cse.ohio-state.edu/benchmarks/). You can build the
benchmarks using the following steps if mpicc is in your PATH. For
example:

If mpicc is not in your path or you would like to use another particular
version you can explicitly tell configure by setting CC. For example:

Configure will detect whether your library supports MPI-2, MPI-3 and
compile the corresponding benchmarks. The benchmarks will be installed
under \$prefix/libexec/osu-micro-benchmarks.

CUDA Extensions to OMB can be enabled by configuring the benchmark suite
with --enable-cuda option as shown below. Similarly, OpenACC Extensions
can be enabled by specifying the --enable-openacc option. The MPI
library used should be able to support MPI communication from buffers in
GPU Device memory.

## Running

The OSU Benchmarks are run in the same manner as other MPI Applications.
The following examples will use mpirun_rsh as the process manager.
Please see
section [run-applications](#running) for more information on running with
other process managers.

### Running OSU Latency and Bandwidth

**Inter-node latency and bandwidth:** The following example will measure
the latency and bandwidth of communication between node1 and node2.

**Intra-node latency and bandwidth:** The following example will measure
the latency and bandwidth of communication inside node1 on different
cores. This assumes that you have at least two cores (or processors) in
your node.

### Running OSU Message Rate Benchmark

The OSU message rate benchmark reports the rate at which messages can be
sent between two nodes. It is advised that it should be run in a
configuration that utilizes multiple pairs of communicating processes on
two nodes. The following example measures the message rate on a system
with two nodes, each with four processor cores.

Where hostfile "mf" has the following contains:

node1\
node1\
node1\
node1\
node2\
node2\
node2\
node2\

### Running OSU Collective Benchmarks

By default, the OSU collective benchmarks report the average
communication latencies for a given collective operation, across various
message lengths. Additionally, the benchmarks offer the following
options:

1.  \"-f\" can be used to report additional statistics, such as min and
    max latencies and the number of iterations.

2.  \"-m\" option can be used to set the maximum message length to be
    used in a benchmark. In the default version, the benchmarks report
    the latencies for up to 1MB message lengths.

3.  \"-i\" can be used to set the number of iterations to run for each
    message length.

4.  \"-h\" can be used to list all the options and their descriptions.

5.  \"-v\" reports the benchmark version.

If a user wishes to measure the communication latency of a specific
collective operation, say, MPI_Alltoall, with 16 processes, we recommend
running the osu_alltoall benchmark in the following manner:

Where hostfile "mf" has the following contains:

node1\
node1\
node1\
node1\
node1\
node1\
node1\
node1\
node1\
node2\
node2\
node2\
node2\
node2\
node2\
node2\
node2\

### Running Benchmarks with CUDA/OpenACC/ROCm Extensions

The following benchmarks have been extended to evaluate performance of
MPI communication from and to buffers on NVIDIA GPU devices.


        -------------------- ----------------------------------------
        osu_bibw             Bidirectional Bandwidth Test
        osu_bw               Bandwidth Test
        osu_latency          Latency Test
        osu_latency_mt       Multi-threaded Latency Test
        osu_mbw_mr           Multiple Bandwidth / Message Rate Test
        osu_multi_lat        Multi-pair Letency Test
        osu_put_latency      Latency Test for Put
        osu_get_latency      Latency Test for Get
        osu_put_bw           Bandwidth Test for Put
        osu_get_bw           Bandwidth Test for Get
        osu_put_bibw         Bidirectional Bandwidth Test for Put
        osu_acc_latency      Latency Test for Accumulate
        osu_cas_latency      Latency Test for Compare and Swap
        osu_fop_latency      Latency Test for Fetch and Op
        osu_allgather        MPI_Allgather Latency Test
        osu_allgatherv       MPI_Allgatherv Latency Test
        osu_allreduce        MPI_Allreduce Latency Test
        osu_alltoall         MPI_Alltoall Latency Test
        osu_alltoallv        MPI_Alltoallv Latency Test
        osu_bcast            MPI_Bcast Latency Test
        osu_gather           MPI_Gather Latency Test
        osu_gatherv          MPI_Gatherv Latency Test
        osu_reduce           MPI_Reduce Latency Test
        osu_reduce_scatter   MPI_Reduce_scatter Latency Test
        osu_scatter          MPI_Scatter Latency Test
        osu_scatterv         MPI_Scatterv Latency Test
        osu_iallgather       MPI_Iallgather Latency Test
        osu_iallreduce       MPI_Iallreduce Latency Test
        osu_ialltoall        MPI_Ialltoall Latency Test
        osu_ibcast           MPI_Ibcast Latency Test
        osu_igather          MPI_Igather Latency Test
        osu_ireduce          MPI_Ireduce Latency Test
        osu_iscatter         MPI_Iscatter Latency Test
        -------------------- ----------------------------------------

Each of the pt2pt benchmarks takes two input parameters. The first
parameter indicates the location of the buffers at rank 0 and the second
parameter indicates the location of the buffers at rank 1. The value of
each of these parameters can be either 'H' or 'D' to indicate if the
buffers are to be on the host or on the device, respectively. When no
parameters are specified, the buffers are allocated on the host.

The collective benchmarks will use buffers allocated on the device if
the -d option is used otherwise the buffers will be allocated on the
host.

If both CUDA and OpenACC support is enabled, you can switch between
using CUDA and OpenACC to allocate your device buffers by specifying the
'-d cuda' or '-d openacc' option to the benchmark. Please use the '-h'
option for more info.

GPU affinity for processes is set before MPI_Init is called. The process
rank on a node is normally used to do this and different MPI launchers
expose this information through different environment variables. The
benchmarks use an environment variable called LOCAL_RANK to get this
information. The script like get_local_rank provided alongside the
benchmarks can be used to export this environment variable when using
mpirun_rsh. This can be adapted to work with other MPI launchers and
libraries.

**Examples:**

In this run, assuming host0 has two GPU devices, the latency test
allocates buffers on GPU device 0 at rank 0 and on GPU device 1 at rank
1.

In this run, the bandwidth test allocates buffers on the GPU device at
rank 0 (host0) and on the host at rank 1 (host1).

### Running Benchmarks with CUDA Managed Memory

The following benchmarks have been extended to evaluate performance of
MPI communication from and to buffers allocated using CUDA Managed
Memory.


        -------------------- ----------------------------------------
        osu_bibw             Bidirectional Bandwidth Test
        osu_bw               Bandwidth Test
        osu_latency          Latency Test
        osu_mbw_mr           Multiple Bandwidth / Message Rate Test
        osu_multi_lat        Multi-pair Letency Test
        osu_allgather        MPI_Allgather Latency Test
        osu_allgatherv       MPI_Allgatherv Latency Test
        osu_allreduce        MPI_Allreduce Latency Test
        osu_alltoall         MPI_Alltoall Latency Test
        osu_alltoallv        MPI_Alltoallv Latency Test
        osu_bcast            MPI_Bcast Latency Test
        osu_gather           MPI_Gather Latency Test
        osu_gatherv          MPI_Gatherv Latency Test
        osu_reduce           MPI_Reduce Latency Test
        osu_reduce_scatter   MPI_Reduce_scatter Latency Test
        osu_scatter          MPI_Scatter Latency Test
        osu_scatterv         MPI_Scatterv Latency Test
        -------------------- ----------------------------------------


In addition to support for communications to and from GPU memories
allocated using CUDA or OpenACC, we now provide additional capability of
performing communications to and from buffers allocated using the CUDA
Managed Memory concept. CUDA Managed (or Unified) Memory allows
applications to allocate memory on either CPU or GPU memories using the
cudaMallocManaged() call. This allows user oblivious transfer of the
memory buffer between the CPU or GPU. Currently, we offer benchmarking
with CUDA Managed Memory using the tests mentioned above. These
benchmarks have additional options: \"M\" allocates a send or receive
buffer as managed for point to point communication. \"-d managed\" uses
managed memory buffers to perform collective communications
