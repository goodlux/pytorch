
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_mkl_Exceptions.h:

Program Listing for File Exceptions.h
=====================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_mkl_Exceptions.h`

.. code-block:: cpp

   #pragma once
   
   #include <string>
   #include <stdexcept>
   #include <sstream>
   #include <mkl_dfti.h>
   
   namespace at { namespace native {
   
   static inline void MKL_DFTI_CHECK(MKL_INT status)
   {
     if (status && !DftiErrorClass(status, DFTI_NO_ERROR)) {
       std::ostringstream ss;
       ss << "MKL FFT error: " << DftiErrorMessage(status);
       throw std::runtime_error(ss.str());
     }
   }
   
   }}  // namespace at::native
