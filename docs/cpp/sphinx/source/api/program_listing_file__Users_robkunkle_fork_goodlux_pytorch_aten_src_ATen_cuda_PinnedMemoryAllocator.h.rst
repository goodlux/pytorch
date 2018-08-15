
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_PinnedMemoryAllocator.h:

Program Listing for File PinnedMemoryAllocator.h
================================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_PinnedMemoryAllocator.h`

.. code-block:: cpp

   #pragma once
   
   #include <ATen/Allocator.h>
   
   namespace at { namespace cuda {
   
   at::Allocator* getPinnedMemoryAllocator();
   
   }} // namespace at::cuda
