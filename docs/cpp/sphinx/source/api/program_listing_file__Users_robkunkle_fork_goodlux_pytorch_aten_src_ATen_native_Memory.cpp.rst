
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_Memory.cpp:

Program Listing for File Memory.cpp
===================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_Memory.cpp`

.. code-block:: cpp

   #include "ATen/ATen.h"
   #include "ATen/Error.h"
   #include "ATen/NativeFunctions.h"
   #include "ATen/detail/CUDAHooksInterface.h"
   
   namespace at {
   namespace native {
   
   Tensor pin_memory(const Tensor& self) {
     if (self.type().backend() != kCPU) {
       AT_ERROR("cannot pin '", self.type().toString(), "' only CPU memory can be pinned");
     }
     auto* allocator = detail::getCUDAHooks().getPinnedMemoryAllocator();
     auto tensor = self.type().tensorWithAllocator(self.sizes(), self.strides(), allocator);
     tensor.copy_(self);
     return tensor;
   }
   
   }
   }
