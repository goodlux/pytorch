
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_OptionsGuard.h:

Program Listing for File OptionsGuard.h
=======================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_OptionsGuard.h`

.. code-block:: cpp

   #pragma once
   
   #include <ATen/Device.h>
   #include <ATen/Layout.h>
   #include <ATen/ScalarType.h>
   #include <ATen/TensorOptions.h>
   #include <ATen/optional.h>
   
   namespace at {
   
   struct DefaultTensorOptions {
     static TensorOptions& get();
   
    private:
     static thread_local at::optional<TensorOptions> options_;
   };
   
   struct OptionsGuard {
     explicit OptionsGuard(const TensorOptions& options)
         : original_(DefaultTensorOptions::get()) {
       DefaultTensorOptions::get() = options;
     }
   
     ~OptionsGuard() {
       DefaultTensorOptions::get() = original_;
     }
   
     const TensorOptions& original() {
       return original_;
     }
   
    private:
     TensorOptions original_;
   };
   
   } // namespace at
