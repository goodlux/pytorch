
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_DimVector.h:

Program Listing for File DimVector.h
====================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_DimVector.h`

.. code-block:: cpp

   #pragma once
   
   #include "SmallVector.h"
   #include <stdint.h>
   
   namespace at {
   
   using DimVector = SmallVector<int64_t, 5>;
   
   }
