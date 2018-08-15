
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Allocator.cpp:

Program Listing for File Allocator.cpp
======================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Allocator.cpp`

.. code-block:: cpp

   #include <ATen/Allocator.h>
   
   namespace at {
   
   static void deleteInefficientStdFunctionContext(void* ptr) {
     delete static_cast<InefficientStdFunctionContext*>(ptr);
   }
   
   at::DataPtr
   InefficientStdFunctionContext::makeDataPtr(void* ptr, const std::function<void(void*)>& deleter, Device device) {
     return {ptr, new InefficientStdFunctionContext({ptr, deleter}), &deleteInefficientStdFunctionContext, device};
   }
   
   } // namespace at
