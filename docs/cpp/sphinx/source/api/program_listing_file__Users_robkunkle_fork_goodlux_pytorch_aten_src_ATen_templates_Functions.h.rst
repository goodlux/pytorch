
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_templates_Functions.h:

Program Listing for File Functions.h
====================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_templates_Functions.h`

.. code-block:: cpp

   #pragma once
   
   // ${generated_comment}
   
   #include "ATen/Scalar.h"
   #include "ATen/Type.h"
   #include "ATen/Tensor.h"
   #include "ATen/Storage.h"
   #include "ATen/Generator.h"
   #include "ATen/Deprecated.h"
   #include "ATen/NativeFunctions.h"
   #include "ATen/DeviceGuard.h"
   #include "ATen/TensorOptions.h"
   #include "THNN/Reduction.h"
   
   namespace at {
   
   using native::from_blob;
   using native::tensor;
   
   ${function_declarations}
   
   static inline Type & infer_type(const Tensor & t) {
     AT_CHECK(t.defined(), "undefined Tensor");
     return t.type();
   }
   static inline Type & infer_type(const TensorList & tl) {
     AT_CHECK(tl.size() > 0, "expected a non-empty list of Tensors");
     return tl[0].type();
   }
   // function definitions are all static inline because
   // they are one-line statically dispatched functions that
   // invoke the actual dynamic dispatch on the correct argument
   ${function_definitions}
   
   }
