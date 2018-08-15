
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_modules_embedding.h:

Program Listing for File embedding.h
====================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_modules_embedding.h`

.. code-block:: cpp

   #pragma once
   
   #include <torch/nn/cloneable.h>
   #include <torch/nn/pimpl.h>
   #include <torch/tensor.h>
   
   #include <cstddef>
   #include <vector>
   
   namespace torch {
   namespace nn {
   
   struct EmbeddingOptions {
     EmbeddingOptions(int64_t count, int64_t dimension);
     TORCH_ARG(int64_t, count);
     TORCH_ARG(int64_t, dimension);
   };
   
   class EmbeddingImpl : public torch::nn::Cloneable<EmbeddingImpl> {
    public:
     EmbeddingImpl(int64_t count, int64_t dimension)
         : EmbeddingImpl(EmbeddingOptions(count, dimension)) {}
     explicit EmbeddingImpl(EmbeddingOptions options);
   
     void reset() override;
     Tensor forward(Tensor);
   
     EmbeddingOptions options;
     Tensor weight;
   };
   
   TORCH_MODULE(Embedding);
   
   } // namespace nn
   } // namespace torch
