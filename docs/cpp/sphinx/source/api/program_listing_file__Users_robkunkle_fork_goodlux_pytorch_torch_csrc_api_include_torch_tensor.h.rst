
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_tensor.h:

Program Listing for File tensor.h
=================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_tensor.h`

.. code-block:: cpp

   #pragma once
   
   #include <ATen/ATen.h>
   
   #include <torch/csrc/autograd/generated/variable_factories.h>
   #include <torch/csrc/autograd/variable.h>
   
   namespace torch {
   using namespace at;
   
   using Tensor = autograd::Variable;
   using Dtype = at::ScalarType;
   
   constexpr auto kUInt8 = at::kByte;
   constexpr auto kInt8 = at::kChar;
   constexpr auto kInt16 = at::kShort;
   constexpr auto kInt32 = at::kInt;
   constexpr auto kInt64 = at::kLong;
   constexpr auto kFloat32 = at::kFloat;
   constexpr auto kFloat64 = at::kDouble;
   
   constexpr auto kU8 = kUInt8;
   constexpr auto kI8 = kInt8;
   constexpr auto kI16 = kInt16;
   constexpr auto kI32 = kInt32;
   constexpr auto kI64 = kInt64;
   constexpr auto kF32 = kFloat32;
   constexpr auto kF64 = kFloat64;
   } // namespace torch
