# MPIRUN_RSH Parameters 

## MV2_COMM_WORLD_LOCAL_RANK 

-   Class: Run time

-   Applicable Interface(s): All

The local rank of a process on a node within its job. The local rank
ranges from 0,1 \... N-1 on a node with N processes running on it.

## MV2_COMM_WORLD_LOCAL_SIZE 

-   Class: Run time

-   Applicable Interface(s): All

The number of ranks from this job that are running on this node.

## MV2_COMM_WORLD_RANK 

-   Class: Run time

-   Applicable Interface(s): All

The MPI rank of this process in current MPI job

## MV2_COMM_WORLD_SIZE 

-   Class: Run time

-   Applicable Interface(s): All

The number of processes in this MPI job's MPI_Comm_World.

## MV2_FASTSSH_THRESHOLD 

-   Class: Run time

-   Default: 800

-   Applicable Interface(s): All

Number of nodes beyond which to use hierarchical ssh during start up.
This parameter is only relevant for mpirun_rsh based start up. Note that
unlike most other parameters described in this section, this is an
environment variable that has to be set in the run time environment (for
e.g. through export in the bash shell).

## MV2_NPROCS_THRESHOLD 

-   Class: Run time

-   Default: 8192

-   Applicable Interface(s): All

Number of processes beyond which to use file-based communication scheme
in the hierarchical ssh during start up. This parameter is only relevant
for mpirun_rsh based start up.

## MV2_MPIRUN_TIMEOUT 

-   Class: Run time

-   Default: Dynamic - based on number of nodes

The number of seconds after which mpirun_rsh aborts job launch. Note
that unlike most other parameters described in this section, this is an
environment variable that has to be set in the run time environment (for
e.g. through export in the bash shell).

## MV2_MT_DEGREE 

-   Class: Run time

-   Default: 32

The degree of the hierarchical tree used by mpirun_rsh. Note that unlike
most other parameters described in this section, this is an environment
variable that has to be set in the run time environment (for e.g.
through export in the bash shell).

## MPIEXEC_TIMEOUT 

-   Class: Run time

-   Default: Unset

-   Unit: Seconds

Set this to limit, in seconds, of the execution time of the mpi
application. This overwrites the\
MV2_MPIRUN_TIMEOUT parameter.

## MV2_DEBUG_FORK_VERBOSE 

-   Class: Run time

-   Type: Null or positive integer

-   Default: 0 (disabled)

Set the verbosity level of the debug output for the process management
operations (fork, waitpid, kill, \...) of mpirun_rsh and mpispawn
processes. The value 0 disables any debug output, a value of 1 enables a
basic debug output, and a value of 2 enables a more verbose debug
output.

Note: All debug output is disabled when MVAPICH2 is configured with the
`–enable-fast=ndebug` option.
