
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Backtrace.h:

Program Listing for File Backtrace.h
====================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Backtrace.h`

.. code-block:: cpp

   #pragma once
   
   #include <cstddef>
   #include <string>
   #include <typeinfo>
   
   #include <ATen/ATenGeneral.h>
   
   namespace at {
   AT_API std::string demangle(const char* name);
   
   template <typename T>
   inline const char* demangle_type() {
   #ifdef __GXX_RTTI
     static const std::string name = demangle(typeid(T).name());
     return name.c_str();
   #else // __GXX_RTTI
     return "(RTTI disabled, cannot show name)";
   #endif // __GXX_RTTI
   }
   
   AT_API std::string get_backtrace(
       size_t frames_to_skip = 0,
       size_t maximum_number_of_frames = 64,
       bool skip_python_frames = true);
   } // namespace at
