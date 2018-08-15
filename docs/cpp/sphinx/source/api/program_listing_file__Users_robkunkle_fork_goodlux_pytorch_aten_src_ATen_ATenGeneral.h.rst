
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_ATenGeneral.h:

Program Listing for File ATenGeneral.h
======================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_ATenGeneral.h`

.. code-block:: cpp

   #pragma once
   
   #ifdef _WIN32
   # if defined(ATen_cpu_EXPORTS) || defined(caffe2_EXPORTS)
   #  define AT_API __declspec(dllexport)
   # else
   #  define AT_API __declspec(dllimport)
   # endif
   #else
   # define AT_API
   #endif
