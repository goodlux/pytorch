
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_test_cudnn_test.cpp:

Program Listing for File cudnn_test.cpp
=======================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_test_cudnn_test.cpp`

.. code-block:: cpp

   #define CATCH_CONFIG_MAIN
   #include "catch.hpp"
   
   #include "ATen/ATen.h"
   #include "ATen/cudnn/Descriptors.h"
   #include "ATen/cudnn/Handles.h"
   #include "test_seed.h"
   
   using namespace at;
   using namespace at::native;
   
   TEST_CASE( "cudnn", "[cuda]" ) {
     manual_seed(123, at::Backend::CUDA);
   
   #if CUDNN_VERSION < 7000
     auto handle = getCudnnHandle();
     DropoutDescriptor desc1, desc2;
     desc1.initialize_rng(at::CUDA(kByte), handle, 0.5, 42);
     desc2.set(handle, 0.5, desc1.state);
   
     REQUIRE(desc1.desc()->dropout == desc2.desc()->dropout);
     REQUIRE(desc1.desc()->nstates == desc2.desc()->nstates);
     REQUIRE(desc1.desc()->states == desc2.desc()->states);
   #endif
   }
