
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_src_nn_modules_embedding.cpp:

Program Listing for File embedding.cpp
======================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_src_nn_modules_embedding.cpp`

.. code-block:: cpp

   #include <torch/nn/modules/embedding.h>
   
   #include <torch/tensor.h>
   
   #include <cstddef>
   #include <utility>
   #include <vector>
   
   namespace torch {
   namespace nn {
   
   EmbeddingOptions::EmbeddingOptions(int64_t count, int64_t dimension)
       : count_(count), dimension_(dimension) {}
   
   EmbeddingImpl::EmbeddingImpl(EmbeddingOptions options)
       : options(std::move(options)) {
     reset();
   }
   
   void EmbeddingImpl::reset() {
     weight = register_parameter(
         "weight", torch::empty({options.count_, options.dimension_}));
     weight.data().normal_(0, 1);
   }
   
   Tensor EmbeddingImpl::forward(Tensor input) {
     return torch::embedding(weight, /*indices=*/input);
   }
   } // namespace nn
   } // namespace torch
