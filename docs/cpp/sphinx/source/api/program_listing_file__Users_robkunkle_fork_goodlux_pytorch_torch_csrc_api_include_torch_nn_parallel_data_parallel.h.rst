
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_parallel_data_parallel.h:

Program Listing for File data_parallel.h
========================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_parallel_data_parallel.h`

.. code-block:: cpp

   #pragma once
   
   #include <torch/cuda.h>
   #include <torch/nn/module.h>
   #include <torch/nn/pimpl.h>
   #include <torch/tensor.h>
   
   #include <torch/csrc/autograd/functions/comm.h>
   #include <torch/csrc/cuda/comm.h>
   
   #include <ATen/Device.h>
   #include <ATen/Error.h>
   #include <ATen/OptionsGuard.h>
   #include <ATen/Parallel.h>
   #include <ATen/TensorOptions.h>
   #include <ATen/optional.h>
   
   #include <cstddef>
   #include <exception>
   #include <memory>
   #include <mutex>
   #include <vector>
   
   namespace torch {
   namespace nn {
   namespace parallel {
   
   template <typename ModuleType>
   std::vector<std::shared_ptr<ModuleType>> replicate(
       const std::shared_ptr<ModuleType>& module,
       const std::vector<Device>& devices) {
     std::vector<std::shared_ptr<ModuleType>> replicas;
     replicas.reserve(devices.size());
     for (const auto& device : devices) {
       replicas.push_back(
           std::static_pointer_cast<ModuleType>(module->clone(device)));
     }
     return replicas;
   }
   
   template <typename ModuleType>
   std::vector<ModuleHolder<ModuleType>> replicate(
       const ModuleHolder<ModuleType>& module,
       const std::vector<Device>& devices) {
     auto ptrs = replicate(module.ptr(), devices);
     return std::vector<ModuleHolder<ModuleType>>(ptrs.begin(), ptrs.end());
   }
   
   template <typename ModuleType>
   std::vector<Tensor> parallel_apply(
       std::vector<ModuleType>& modules,
       const std::vector<Tensor>& inputs,
       const at::optional<std::vector<Device>>& devices = at::nullopt) {
     AT_CHECK(
         modules.size() == inputs.size(), "Must have as many inputs as modules");
     if (devices) {
       AT_CHECK(
           modules.size() == devices->size(),
           "Must have as many devices as modules");
     }
   
     std::vector<Tensor> outputs(modules.size());
     std::mutex mutex;
   
     // std::exception_ptr can be passed between threads:
     // > An instance of std::exception_ptr may be passed to another function,
     // > possibly on another thread, where the exception may be rethrown [...].
     // https://en.cppreference.com/w/cpp/error/exception_ptr
     std::exception_ptr exception;
   
     at::parallel_for(
         /*begin=*/0,
         /*end=*/modules.size(),
         /*grain_size=*/1,
         [&modules, &inputs, &devices, &outputs, &mutex, &exception](
             int64_t index, int64_t stop) {
           for (; index < stop; ++index) {
             try {
               torch::OptionsGuard options_guard(
                   devices ? (*devices)[index] : inputs[index].device());
               auto output = modules[index]->forward(inputs[index]);
               std::lock_guard<std::mutex> lock(mutex);
               outputs[index] = output;
             } catch (...) {
               std::lock_guard<std::mutex> lock(mutex);
               if (!exception) {
                 exception = std::current_exception();
               }
             }
           }
         });
   
     if (exception) {
       std::rethrow_exception(exception);
     }
   
     return outputs;
   }
   
   template <typename ModuleType>
   Tensor data_parallel(
       ModuleType module,
       Tensor input,
       at::optional<std::vector<Device>> devices = at::nullopt,
       at::optional<Device> output_device = at::nullopt,
       int64_t dim = 0) {
     if (!devices) {
       const auto device_count = torch::cuda::device_count();
       AT_CHECK(device_count > 0, "Expected at least one CUDA device");
       devices.emplace();
       devices->reserve(device_count);
       for (size_t index = 0; index < device_count; ++index) {
         devices->emplace_back(kCUDA, index);
       }
     }
     if (!output_device) {
       output_device = devices->front();
     }
   
     if (devices->size() == 1) {
       OptionsGuard guard(devices->front());
       return module->forward(std::move(input)).to(*output_device);
     }
   
   #ifdef USE_CUDA
     autograd::Scatter scatter(*devices, /*chunk_sizes=*/at::nullopt, dim);
     auto scattered_inputs = scatter.apply({std::move(input)});
   
     auto replicas = replicate(module, *devices);
     auto outputs = parallel_apply(replicas, scattered_inputs, *devices);
     return autograd::Gather(*output_device, dim)
         .apply(std::move(outputs))
         .front();
   #else
     AT_ERROR("data_parallel not supported without CUDA");
   #endif
   }
   
   } // namespace parallel
   } // namespace nn
   } // namespace torch
