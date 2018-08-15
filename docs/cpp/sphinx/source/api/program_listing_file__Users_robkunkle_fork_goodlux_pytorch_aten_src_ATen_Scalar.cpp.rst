
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Scalar.cpp:

Program Listing for File Scalar.cpp
===================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Scalar.cpp`

.. code-block:: cpp

   #include "ATen/Config.h"
   
   #include "ATen/Scalar.h"
   
   #include <TH/TH.h>
   
   #include "ATen/Tensor.h"
   #include "ATen/Context.h"
   
   namespace at {
   Tensor Scalar::toTensor() const {
     if (Tag::HAS_t == tag) {
       return Tensor(t);
     } else if (Tag::HAS_d == tag) {
       return CPU(kDouble).scalarTensor(*this);
     } else {
       assert(Tag::HAS_i == tag);
       return CPU(kLong).scalarTensor(*this);
     }
   }
   }
