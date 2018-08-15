
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_templates_RegisterCUDA.cpp:

Program Listing for File RegisterCUDA.cpp
=========================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_templates_RegisterCUDA.cpp`

.. code-block:: cpp

   #include <ATen/RegisterCUDA.h>
   
   // ${generated_comment}
   
   #include <ATen/Type.h>
   #include <ATen/Context.h>
   #include <ATen/detail/VariableHooksInterface.h>
   
   ${cuda_type_headers}
   
   namespace at {
   
   void register_cuda_types(Context * context) {
     ${cuda_type_registrations}
   }
   
   } // namespace at
