
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Utils.cpp:

Program Listing for File Utils.cpp
==================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Utils.cpp`

.. code-block:: cpp

   #include "ATen/Utils.h"
   #include <stdarg.h>
   #include <stdexcept>
   #include <typeinfo>
   #include <cstdlib>
   
   namespace at {
   
   int _crash_if_asan(int arg) {
     volatile char x[3];
     x[arg] = 0;
     return x[0];
   }
   
   } // at
