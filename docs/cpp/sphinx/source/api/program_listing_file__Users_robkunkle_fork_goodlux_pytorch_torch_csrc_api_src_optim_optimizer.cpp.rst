
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_src_optim_optimizer.cpp:

Program Listing for File optimizer.cpp
======================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_src_optim_optimizer.cpp`

.. code-block:: cpp

   #include <torch/optim/optimizer.h>
   
   #include <torch/nn/cursor.h>
   #include <torch/tensor.h>
   
   #include <utility>
   #include <vector>
   
   namespace torch {
   namespace optim {
   namespace detail {
   
   OptimizerBase::OptimizerBase(std::vector<Tensor> parameters)
       : parameters_(std::move(parameters)) {}
   
   OptimizerBase::OptimizerBase(const ParameterCursor& cursor) {
     add_parameters(cursor);
   }
   
   void OptimizerBase::add_parameters(const std::vector<Tensor>& parameters) {
     parameters_.insert(parameters_.end(), parameters.begin(), parameters.end());
   }
   
   void OptimizerBase::add_parameters(const ParameterCursor& cursor) {
     std::vector<Tensor> tensors(cursor.size());
     cursor.map(tensors.begin(), [](const Tensor& tensor) { return tensor; });
     add_parameters(tensors);
   }
   
   void OptimizerBase::zero_grad() {
     for (auto& parameter : parameters_) {
       auto& grad = parameter.grad();
       if (grad.defined()) {
         grad = grad.detach();
         Tensor(grad).data().zero_();
       }
     }
   }
   
   size_t OptimizerBase::size() const noexcept {
     return parameters_.size();
   }
   } // namespace detail
   } // namespace optim
   } // namespace torch
