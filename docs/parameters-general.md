# MVAPICH2 General Parameters 

## MV2_IGNORE_SYSTEM_CONFIG 

-   Class: Run time

-   Default: 0

If set, the system configuration file is not processed.

## MV2_IGNORE_USER_CONFIG 

-   Class: Run time

-   Default: 0

If set, the user configuration file is not processed.

## MV2_USER_CONFIG 

-   Class: Run time

-   Default: Unset

Specify the path of a user configuration file for mvapich2. If this is
not set the default path of " /.mvapich2.conf" is used.

## MV2_DEBUG_CORESIZE 

-   Class: Run time

-   Default: Unset

-   Possible values: Positive integer or \"unlimited\"

Set the limit for the core size resource. It allows to specify the
maximum size for a core dump to be generated. It only set the soft limit
and it has the respect the hard value set on the nodes.

It is similar to the `ulimit -c <coresize>` that can be run in the
shell, but this will only apply to the MVAPICH2 processes (MPI
processes, mpirun_rsh, mpispawn).

Examples:

-   '`MV2_DEBUG_CORESIZE=0`' will disable core dumps for MVAPICH2
    processes.

-   '`MV2_DEBUG_CORESIZE=unlimited`' will enable core dumps for MVAPICH2
    processes.

## MV2_DEBUG_SHOW_BACKTRACE 

-   Class: Run time

-   Default: 0 (disabled)

-   Possible values: 1 to enable, 0 to disable

Show a backtrace when a process fails on errors like \"Segmentation
faults\", \"Bus error\", \"Illegal Instruction\", \"Abort\" or
\"Floating point exception\".

If your application uses the static version of the MVAPICH2 library, you
have to link your application with the `-rdynamic` flag in order to see
the function names in the backtrace. For more information, see the
backtrace manpage.

## MV2_SHOW_ENV_INFO 

-   Class: Run time

-   Default: 0 (disabled)

-   Possible values: 1 (short list), 2(full list)

Show the values assigned to the run time environment parameters

## MV2_SHOW_CPU_BINDING 

-   Class: Run time

-   Default: 0 (disabled)

-   Possible values: 1 (Show only on node containing rank 0), 2 (Show on
    all nodes)

If set to 1, it shows the CPU mapping of all processes on node where
rank 0 exists. If set to 2, it shows the CPU mapping of all processes on
all nodes.
