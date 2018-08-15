
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_utils.h:

Program Listing for File utils.h
================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_utils.h`

.. code-block:: cpp

   #pragma once
   
   #include <torch/csrc/autograd/grad_mode.h>
   
   #include <cstdint>
   
   namespace torch {
   using autograd::AutoGradMode;
   
   // A RAII, thread local (!) guard that stops future operations from building
   // gradients.
   struct NoGradGuard : public AutoGradMode {
     NoGradGuard() : AutoGradMode(/*enabled=*/false) {}
   };
   
   void manual_seed(uint64_t seed);
   } // namespace torch
