
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_src_utils.cpp:

Program Listing for File utils.cpp
==================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_src_utils.cpp`

.. code-block:: cpp

   #include <torch/utils.h>
   
   #include <ATen/Context.h>
   
   #include <cstddef>
   
   namespace torch {
   void manual_seed(uint64_t seed) {
     // TODO: Move this to at::Context
     at::globalContext().defaultGenerator(at::Backend::CPU).manualSeed(seed);
     if (at::globalContext().hasCUDA()) {
       at::globalContext().defaultGenerator(at::Backend::CUDA).manualSeedAll(seed);
     }
   }
   } // namespace torch
