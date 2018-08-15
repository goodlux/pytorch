
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_cpu_SoftmaxKernel.h:

Program Listing for File SoftmaxKernel.h
========================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_cpu_SoftmaxKernel.h`

.. code-block:: cpp

   #pragma once
   
   #include <ATen/ATen.h>
   #include <ATen/native/DispatchStub.h>
   
   namespace at {
   namespace native {
   
   using forward_fn = void(*)(Tensor &, const Tensor &);
   using backward_fn = void(*)(Tensor &, const Tensor &, const Tensor&);
   
   DECLARE_DISPATCH(forward_fn, softmax_lastdim_kernel);
   DECLARE_DISPATCH(forward_fn, log_softmax_lastdim_kernel);
   DECLARE_DISPATCH(backward_fn, softmax_backward_lastdim_kernel);
   DECLARE_DISPATCH(backward_fn, log_softmax_backward_lastdim_kernel);
   
   }
   }
