
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_mkl_Limits.h:

Program Listing for File Limits.h
=================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_mkl_Limits.h`

.. code-block:: cpp

   #pragma once
   
   #include <mkl_types.h>
   
   namespace at { namespace native {
   
     // Since size of MKL_LONG varies on different platforms (linux 64 bit, windows
     // 32 bit), we need to programmatically calculate the max.
     static int64_t MKL_LONG_MAX = ((1LL << (sizeof(MKL_LONG) * 8 - 2)) - 1) * 2 + 1;
   
   }}  // namespace
