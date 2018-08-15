
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_test_test_seed.h:

Program Listing for File test_seed.h
====================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_test_test_seed.h`

.. code-block:: cpp

   #pragma once
   
   #include "ATen/ATen.h"
   
   void manual_seed(uint64_t seed, at::Backend backend) {
     if (backend == at::Backend::CPU) {
       at::Generator & cpu_gen = at::globalContext().defaultGenerator(at::Backend::CPU);
       cpu_gen.manualSeed(seed);
     } else if (backend == at::Backend::CUDA && at::hasCUDA()) {
       at::Generator & cuda_gen = at::globalContext().defaultGenerator(at::Backend::CUDA);
       cuda_gen.manualSeed(seed);
     }
   }
