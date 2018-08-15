
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_Vision.cpp:

Program Listing for File Vision.cpp
===================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_Vision.cpp`

.. code-block:: cpp

   #include "ATen/ATen.h"
   #include "ATen/NativeFunctions.h"
   #include "ATen/detail/CUDAHooksInterface.h"
   
   namespace {
     enum GridSamplerMode {GridSamplerModeZeros, GridSamplerModeBorder};
   }
   
   namespace at { namespace native {
   
   Tensor grid_sampler(const Tensor& input, const Tensor& grid, int64_t padding_mode) {
     // cudnn does not support inputs larger than 1024
     if (at::native::cudnn_is_acceptable(input) &&
         padding_mode == GridSamplerModeZeros &&
         input.dim() == 4 &&
         input.size(1) <= 1024) {
       return cudnn_grid_sampler(input, grid);
     }
     if (input.dim() == 4) {
       return thnn_grid_sampler_bilinear2d(input, grid, padding_mode);
     }
     if (input.dim() == 5) {
       return thnn_grid_sampler_bilinear3d(input, grid, padding_mode);
     }
     AT_ERROR("grid_sampler(): input must be 4d or 5d but got input of shape: ", input.dim());
   }
   
   }}  // namespace at::native
