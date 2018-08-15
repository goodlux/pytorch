
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_test_verify_api_visibility.cpp:

Program Listing for File verify_api_visibility.cpp
==================================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_test_verify_api_visibility.cpp`

.. code-block:: cpp

   #include <ATen/ATen.h>
   
   #ifdef AT_CUDNN_ENABLED
   #error "AT_CUDNN_ENABLED should not be visible in public headers"
   #endif
   
   #ifdef AT_MKL_ENABLED
   #error "AT_MKL_ENABLED should not be visible in public headers"
   #endif
   
   #ifdef AT_MKLDNN_ENABLED
   #error "AT_MKLDNN_ENABLED should not be visible in public headers"
   #endif
   
   auto main() -> int {}
