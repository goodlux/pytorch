
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_module.h:

Program Listing for File module.h
=================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_module.h`

.. code-block:: cpp

   #pragma once
   
   #include <torch/detail/ordered_dict.h>
   #include <torch/nn/cursor.h>
   #include <torch/nn/pimpl.h>
   #include <torch/tensor.h>
   
   #include <ATen/ATen.h>
   #include <ATen/optional.h>
   
   #include <map>
   #include <memory>
   #include <string>
   #include <type_traits>
   #include <unordered_map>
   
   namespace torch {
   namespace detail {
   template <typename T>
   class CursorBase;
   } // namespace detail
   } // namespace torch
   
   namespace torch {
   namespace nn {
   
   class Module {
    public:
     explicit Module(std::string name);
   
     Module() = default;
   
     virtual ~Module() = default;
   
     const std::string& name() const noexcept;
   
     virtual std::shared_ptr<Module> clone(
         at::optional<Device> device = at::nullopt) const;
   
     ModuleCursor modules();
     ConstModuleCursor modules() const;
   
     ModuleCursor children();
     ConstModuleCursor children() const;
   
     ParameterCursor parameters();
     ConstParameterCursor parameters() const;
   
     BufferCursor buffers();
     ConstBufferCursor buffers() const;
   
     virtual void train();
   
     virtual void eval();
   
     virtual bool is_training() const noexcept;
   
     virtual void to(
         torch::Device device,
         torch::Dtype dtype,
         bool non_blocking = false);
   
     virtual void to(torch::Dtype dtype, bool non_blocking = false);
   
     virtual void to(torch::Device device, bool non_blocking = false);
   
     virtual void zero_grad();
   
     template <class Archive>
     void save(Archive& ar) const;
   
     template <class Archive>
     void load(Archive& ar);
   
     template <typename ModuleType>
     typename ModuleType::ContainedType* as() noexcept;
   
     template <
         typename ModuleType,
         typename = torch::detail::disable_if_module_holder_t<ModuleType>>
     ModuleType* as() noexcept;
   
    protected:
     Tensor& register_parameter(
         std::string name,
         Tensor tensor,
         bool requires_grad = true);
     Tensor& register_buffer(std::string name, Tensor tensor);
   
     template <typename ModuleType>
     std::shared_ptr<ModuleType> register_module(
         std::string name,
         std::shared_ptr<ModuleType> module);
   
     template <typename ModuleType>
     std::shared_ptr<ModuleType> register_module(
         std::string name,
         ModuleHolder<ModuleType> module_holder);
   
    private:
     template <typename T>
     using OrderedDict = torch::detail::OrderedDict<std::string, T>;
   
     template <typename Derived>
     friend class Cloneable;
     template <typename T>
     friend class detail::CursorBase;
   
     virtual void clone_(Module& other, at::optional<Device> device);
   
     template <typename... Ts>
     void to_impl(Ts&&... ts);
   
     OrderedDict<Tensor> parameters_;
     OrderedDict<Tensor> buffers_;
     OrderedDict<std::shared_ptr<Module>> children_;
   
     mutable at::optional<std::string> name_;
   
     bool is_training_{true};
   };
   
   // ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ nn::Module ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
   
   template <class Archive>
   void Module::save(Archive& ar) const {
     auto params = parameters();
     size_t size = params.size();
     ar(size);
     for (auto& p : params) {
       ar(p.key, p.value);
     }
   }
   
   template <class Archive>
   void Module::load(Archive& ar) {
     auto params = parameters();
     size_t size;
     ar(size);
     std::string name;
     for (size_t i = 0; i < size; i++) {
       ar(name);
       ar(params[name]);
     }
   }
   
   template <typename ModuleType>
   typename ModuleType::ContainedType* Module::as() noexcept {
     // Use the contained type of the `ModuleHolder`, e.g. `LinearImpl` for
     // `Linear`, since `LinearImpl` inherits `nn::Module`.
     return as<typename ModuleType::ContainedType>();
   }
   
   template <typename ModuleType, typename>
   ModuleType* Module::as() noexcept {
     return dynamic_cast<ModuleType*>(this);
   }
   
   template <typename ModuleType>
   std::shared_ptr<ModuleType> Module::register_module(
       std::string name,
       std::shared_ptr<ModuleType> module) {
     auto& base_module = children_.insert(std::move(name), std::move(module));
     return std::static_pointer_cast<ModuleType>(base_module);
   }
   
   template <typename ModuleType>
   std::shared_ptr<ModuleType> Module::register_module(
       std::string name,
       ModuleHolder<ModuleType> module_holder) {
     return register_module(std::move(name), module_holder.ptr());
   }
   
   template <typename... Ts>
   void Module::to_impl(Ts&&... ts) {
     // First call `to()` on every child module.
     for (auto& child : children_) {
       child.value->to(ts...);
     }
     // Then move every parameter to the new dtype/device.
     for (auto& parameter : parameters_) {
       at::detail::set_data(*parameter, parameter->data().to(ts...));
     }
     // Then move every buffer to the new dtype/device.
     for (auto& buffer : buffers_) {
       at::detail::set_data(*buffer, buffer->data().to(ts...));
     }
   }
   
   } // namespace nn
   } // namespace torch
