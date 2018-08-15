
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Tensor.cpp:

Program Listing for File Tensor.cpp
===================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Tensor.cpp`

.. code-block:: cpp

   #include <ATen/ATen.h>
   
   #include <iostream>
   
   namespace at {
   
   void Tensor::print() const {
     if (defined()) {
       std::cerr << "[" << type().toString() << " " << sizes() << "]" << std::endl;
     } else {
       std::cerr << "[UndefinedTensor]" << std::endl;
     }
   }
   } // namespace at
