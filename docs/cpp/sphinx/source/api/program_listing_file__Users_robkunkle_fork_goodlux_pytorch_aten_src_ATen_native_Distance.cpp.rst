
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_Distance.cpp:

Program Listing for File Distance.cpp
=====================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_Distance.cpp`

.. code-block:: cpp

   #include "ATen/ATen.h"
   #include "ATen/NativeFunctions.h"
   
   
   namespace at { namespace native {
   
   Tensor pairwise_distance(const Tensor& x1, const Tensor& x2, double p, double eps, bool keepdim) {
     return at::norm(x1 - x2 + eps, p, 1, keepdim);
   }
   }}  // namespace at::native
