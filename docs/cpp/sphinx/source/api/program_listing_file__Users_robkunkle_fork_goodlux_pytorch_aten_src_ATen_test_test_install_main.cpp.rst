
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_test_test_install_main.cpp:

Program Listing for File main.cpp
=================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_test_test_install_main.cpp`

.. code-block:: cpp

   #include "ATen/ATen.h"
   
   int main() {
     std::cout << at::ones({3,4}, at::CPU(at::kFloat)) << "\n";
   }
