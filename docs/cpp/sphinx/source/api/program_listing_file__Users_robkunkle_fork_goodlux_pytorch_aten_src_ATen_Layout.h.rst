
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Layout.h:

Program Listing for File Layout.h
=================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Layout.h`

.. code-block:: cpp

   #pragma once
   
   #include <ATen/ScalarType.h>
   #include <ATen/Error.h>
   
   #include <iostream>
   
   namespace at {
   enum class Layout { Strided, Sparse };
   
   constexpr auto kStrided = Layout::Strided;
   constexpr auto kSparse = Layout::Sparse;
   
   inline Layout layout_from_backend(Backend backend) {
     switch (backend) {
       case Backend::SparseCPU:
       case Backend::SparseCUDA:
         return Layout::Sparse;
       default:
         return Layout::Strided;
     }
   }
   } // namespace at
   
   inline std::ostream& operator<<(std::ostream& stream, at::Layout layout) {
     switch (layout) {
       case at::kStrided:
         return stream << "Strided";
       case at::kSparse:
         return stream << "Sparse";
       default:
         AT_ERROR("Unknown layout");
     }
   }
