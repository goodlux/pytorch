
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_DLConvertor.h:

Program Listing for File DLConvertor.h
======================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_DLConvertor.h`

.. code-block:: cpp

   #pragma once
   
   #include "ATen/Tensor.h"
   #include "ATen/ATen.h"
   #include "ATen/dlpack.h"
   
   // this convertor will:
   // 1) take a Tensor object and wrap it in the DLPack tensor
   // 2) take a dlpack tensor and convert it to the ATen Tensor
   
   namespace at {
   
   AT_API ScalarType toScalarType(const DLDataType& dtype);
   AT_API DLManagedTensor * toDLPack(const Tensor& src);
   AT_API Tensor fromDLPack(const DLManagedTensor* src);
   
   } //namespace at
