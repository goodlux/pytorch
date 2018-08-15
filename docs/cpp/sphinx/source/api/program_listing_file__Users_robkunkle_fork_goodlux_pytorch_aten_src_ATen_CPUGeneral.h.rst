
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_CPUGeneral.h:

Program Listing for File CPUGeneral.h
=====================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_CPUGeneral.h`

.. code-block:: cpp

   #pragma once
   
   // Using AT_API is crucial as otherwise you'll see
   // linking errors using MSVC
   // See https://msdn.microsoft.com/en-us/library/a90k134d.aspx
   // This header adds this if using AT_API
   #include "ATen/ATenGeneral.h"
   
   namespace at {
   AT_API void set_num_threads(int);
   AT_API int get_num_threads();
   }
