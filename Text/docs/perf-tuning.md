# Performance Tuning 

MVAPICH2 supports many different parameters for tuning performance for a
wide variety of applications. These parameters can be either compile
time parameters or run time parameters.

In this section we classify these parameters depending on what you are
tuning for and provide guidelines on how to use them.

## Two-sided Point-to-point Tuning

Two-sided point-to-point latency and bandwidth can be tuned very simply
by using the parameters RDMA_IBA_EAGER_THRESHOLD (run time parameter)
and NUM_RDMA_BUFFER (run time parameter).

Messages larger than RDMA_IBA_EAGER_THRESHOLD will go over the
Rendezvous protocol using zero copy. While this can reduce the number of
copies, it can be costly for small messages.

NUM_RDMA_BUFFER indicates the number of RDMA buffers per connection. If
this parameter is increased, more outstanding messages can be
transferred by using the fast path. However, increasing this parameter
also leads to increased memory usage.

## One-sided Point-to-point Tuning

One-sided point-to-point performance can be tuned by using the following
parameters:

-   RDMA_PUT_FALLBACK_THRESHOLD (run time parameter)

-   RDMA_GET_FALLBACK_THRESHOLD (run time parameter)

-   RDMA_EAGERSIZE_1SC (run time parameter)

These parameters take effect only if MVAPICH2 is configured with
-D_1SC\_ flag.

MVAPICH2 uses two schemes for one sided communication. One is
implemented on top of point to point communication (RDMA channel) and
the other is direct one sided implementation (extension of CH3
interface). The RDMA_PUT_FALLBACK_THRESHOLD variable is used to define
the switch point between these two schemes for MPI_PUT messages.
Messages with size smaller than this threshold will use point-to-point
scheme. Similarly, RDMA_GET_FALLBACK_THRESHOLD defines the switch point
between these two schemes for MPI_GET messages.

One sided implementation will either do copy-and-send or register user
buffer on the fly when sending messages. The variable RDMA_EAGERSIZE_1SC
defines the switch point between these two schemes. Please note that if
the value of RDMA_EAGERSIZE_1SC is smaller than\
RDMA_PUT_FALLBACK_THRESHOLD or RDMA_GET_FALLBACK_THRESHOLD, it will take
no effect.

## Tuning Memory Usage

Memory usage often plays a significant role in application performance,
and especially more so for large scale clusters. The main parameters
which decide the memory usage are :

-   VBUF_TOTAL_SIZE (compile time parameter)

-   NUM_RDMA_BUFFER (run time parameter)

The product of VBUF_TOTAL_SIZE and NUM_RDMA_BUFFER generally is a
measure of the amount of memory to be pinned down for eager message
passing. These buffers are not shared across connections.
