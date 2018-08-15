
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_OptionsGuard.cpp:

Program Listing for File OptionsGuard.cpp
=========================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_OptionsGuard.cpp`

.. code-block:: cpp

   #include <ATen/OptionsGuard.h>
   #include <ATen/optional.h>
   
   namespace at {
   
   thread_local at::optional<TensorOptions> DefaultTensorOptions::options_;
   
   TensorOptions& DefaultTensorOptions::get() {
     if (!options_) {
       options_.emplace(
           /*use_thread_local_default_options=*/false);
     }
     return *options_;
   }
   
   } // namespace at
