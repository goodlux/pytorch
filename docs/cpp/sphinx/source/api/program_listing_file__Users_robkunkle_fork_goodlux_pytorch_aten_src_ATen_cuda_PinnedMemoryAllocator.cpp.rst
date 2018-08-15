
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_PinnedMemoryAllocator.cpp:

Program Listing for File PinnedMemoryAllocator.cpp
==================================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_PinnedMemoryAllocator.cpp`

.. code-block:: cpp

   #include <ATen/cuda/PinnedMemoryAllocator.h>
   #include <ATen/Context.h>
   #include <ATen/Config.h>
   
   #include <THC/THC.h>
   #include <THC/THCGeneral.hpp>
   
   #include <stdexcept>
   
   namespace at { namespace cuda {
   
   at::Allocator* getPinnedMemoryAllocator() {
     auto state = globalContext().lazyInitCUDA();
     return state->cudaHostAllocator;
   }
   
   }} // namespace at::cuda
