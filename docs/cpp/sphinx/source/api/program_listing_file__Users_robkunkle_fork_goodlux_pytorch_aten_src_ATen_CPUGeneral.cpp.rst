
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_CPUGeneral.cpp:

Program Listing for File CPUGeneral.cpp
=======================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_CPUGeneral.cpp`

.. code-block:: cpp

   #include <ATen/CPUGeneral.h>
   #include <atomic>
   #include <memory>
   #include <thread>
   
   namespace at {
   // Lock free atomic type
   std::atomic<int> num_threads(-1);
   
   void set_num_threads(int num_threads_) {
     if (num_threads_ >= 0)
       num_threads.store(num_threads_);
   }
   
   int get_num_threads() { return num_threads.load(); }
   }
