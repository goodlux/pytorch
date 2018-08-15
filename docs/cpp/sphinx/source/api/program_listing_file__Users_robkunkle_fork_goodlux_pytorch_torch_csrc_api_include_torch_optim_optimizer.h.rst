
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_optim_optimizer.h:

Program Listing for File optimizer.h
====================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_optim_optimizer.h`

.. code-block:: cpp

   #pragma once
   
   #include <torch/csrc/autograd/generated/variable_factories.h>
   #include <torch/nn/cursor.h>
   #include <torch/tensor.h>
   
   #include <functional>
   #include <memory>
   #include <vector>
   
   namespace torch {
   namespace optim {
   namespace detail {
   
   class OptimizerBase {
    public:
     using ParameterCursor = torch::detail::CursorBase<Tensor>;
   
     explicit OptimizerBase(std::vector<Tensor> parameters);
   
     explicit OptimizerBase(const ParameterCursor& cursor);
   
     virtual ~OptimizerBase() = default;
   
     virtual void add_parameters(const std::vector<Tensor>& parameters);
   
     virtual void add_parameters(const ParameterCursor& cursor);
   
     virtual void zero_grad();
   
     const std::vector<Tensor>& parameters() const noexcept;
   
     size_t size() const noexcept;
   
    protected:
     OptimizerBase() = default;
   
     template <typename ParameterContainer>
     std::vector<Tensor> zero_buffers_like(const ParameterContainer& parameters) {
       std::vector<Tensor> result;
       result.reserve(parameters.size());
       for (auto& parameter : parameters) {
         result.push_back(torch::zeros_like(parameter));
       }
       return result;
     }
   
     Tensor& buffer_at(std::vector<Tensor>& buffers, size_t index) {
       const auto& parameter = parameters_.at(index);
       const auto& buffer = buffers.at(index);
       if (buffer.device() != parameter.device() ||
           buffer.dtype() != parameter.dtype()) {
         buffers[index] = buffer.to(parameter.device(), parameter.dtype());
       }
       return buffers[index];
     }
   
     std::vector<Tensor> parameters_;
   };
   } // namespace detail
   
   class Optimizer : public detail::OptimizerBase {
    public:
     using detail::OptimizerBase::OptimizerBase;
     virtual void step() = 0;
   };
   
   class LossClosureOptimizer : public detail::OptimizerBase {
    public:
     using LossClosure = std::function<Tensor()>;
     using detail::OptimizerBase::OptimizerBase;
     virtual Tensor step(LossClosure closure) = 0;
   };
   
   } // namespace optim
   } // namespace torch
