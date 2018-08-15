
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_cloneable.h:

Program Listing for File cloneable.h
====================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_cloneable.h`

.. code-block:: cpp

   #pragma once
   
   #include <torch/nn/module.h>
   #include <torch/tensor.h>
   
   #include <ATen/Error.h>
   #include <ATen/OptionsGuard.h>
   #include <ATen/TensorOptions.h>
   #include <ATen/optional.h>
   
   #include <memory>
   #include <utility>
   
   namespace torch {
   namespace nn {
   template <typename Derived>
   class Cloneable : public Module {
    public:
     using Module::Module;
   
     virtual void reset() = 0;
   
     std::shared_ptr<Module> clone(
         at::optional<Device> device = at::nullopt) const override {
       auto options = DefaultTensorOptions::get();
       OptionsGuard options_guard(
           options.device(device.value_or(options.device())));
   
       const auto& self = static_cast<const Derived&>(*this);
       auto copy = std::make_shared<Derived>(self);
       copy->parameters_.clear();
       copy->buffers_.clear();
       copy->children_.clear();
       copy->reset();
       AT_CHECK(
           copy->parameters_.size() == parameters_.size(),
           "The cloned module does not have the same number of "
           "parameters as the original module after calling reset(). "
           "Are you sure you called register_parameter() inside reset() "
           "and not the constructor?");
       for (const auto& parameter : parameters_) {
         if (device) {
           copy->parameters_[parameter.key].data().copy_(
               parameter->data(), /*non_blocking=*/true);
         } else {
           at::detail::set_data(
               copy->parameters_[parameter.key], parameter->data().clone());
         }
       }
       AT_CHECK(
           copy->buffers_.size() == buffers_.size(),
           "The cloned module does not have the same number of "
           "buffers as the original module after calling reset(). "
           "Are you sure you called register_buffer() inside reset() "
           "and not the constructor?");
       for (const auto& buffer : buffers_) {
         if (device) {
           copy->buffers_[buffer.key].data().copy_(
               buffer->data(), /*non_blocking=*/true);
         } else {
           at::detail::set_data(
               copy->buffers_[buffer.key], buffer->data().clone());
         }
       }
       AT_CHECK(
           copy->children_.size() == children_.size(),
           "The cloned module does not have the same number of "
           "child modules as the original module after calling reset(). "
           "Are you sure you called register_module() inside reset() "
           "and not the constructor?");
       for (const auto& child : children_) {
         copy->children_[child.key]->clone_(*child.value, device);
       }
       return copy;
     }
   
    private:
     void clone_(Module& other, at::optional<Device> device) final override {
       // Here we are *pretty* certain that `other's` type is `Derived` (because it
       // was registered under the same name as `this`), but you never know what
       // crazy things `reset()` does, so `dynamic_cast` just to be safe.
       auto clone = std::dynamic_pointer_cast<Derived>(other.clone(device));
       AT_CHECK(
           clone != nullptr,
           "Attempted to clone submodule, but it is of a "
           "different type than the submodule it was to be cloned into");
       static_cast<Derived&>(*this) = std::move(*clone);
     }
   };
   
   } // namespace nn
   } // namespace torch
