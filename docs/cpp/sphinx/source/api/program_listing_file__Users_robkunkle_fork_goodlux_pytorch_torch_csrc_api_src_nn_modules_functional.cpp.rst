
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_src_nn_modules_functional.cpp:

Program Listing for File functional.cpp
=======================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_src_nn_modules_functional.cpp`

.. code-block:: cpp

   #include <torch/nn/modules/functional.h>
   
   #include <torch/tensor.h>
   
   #include <functional>
   #include <utility>
   
   namespace torch {
   namespace nn {
   FunctionalImpl::FunctionalImpl(std::function<Tensor(Tensor)> function)
       : function_(std::move(function)) {}
   
   void FunctionalImpl::reset() {}
   
   Tensor FunctionalImpl::forward(Tensor input) {
     return function_(input);
   }
   
   Tensor FunctionalImpl::operator()(Tensor input) {
     return forward(input);
   }
   } // namespace nn
   } // namespace torch
