
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_test_cuda_rng_test.cpp:

Program Listing for File cuda_rng_test.cpp
==========================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_test_cuda_rng_test.cpp`

.. code-block:: cpp

   #define CATCH_CONFIG_MAIN
   #include "catch.hpp"
   
   #include "ATen/ATen.h"
   #include "cuda.h"
   #include "cuda_runtime.h"
   #include <thread>
   
   void makeRandomNumber() {
     cudaSetDevice(std::rand() % 2);
     auto x = at::randn({1000});
   }
   
   void testCudaRNGMultithread() {
     auto threads = std::vector<std::thread>();
     for (auto i = 0; i < 1000; i++) {
       threads.emplace_back(makeRandomNumber);
     }
     for (auto& t : threads) {
       t.join();
     }
   };
   
   TEST_CASE( "CUDA RNG test", "[cuda]" ) {
     SECTION( "multithread" )
       testCudaRNGMultithread();
   }
