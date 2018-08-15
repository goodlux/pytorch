
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_modules_any.h:

Program Listing for File any.h
==============================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_modules_any.h`

.. code-block:: cpp

   #pragma once
   
   #include <torch/detail/static.h>
   #include <torch/nn/module.h>
   #include <torch/nn/pimpl.h>
   #include <torch/tensor.h>
   
   #include <torch/csrc/utils/memory.h>
   #include <torch/csrc/utils/variadic.h>
   
   #include <ATen/Device.h>
   #include <ATen/optional.h>
   
   #include <memory>
   #include <type_traits>
   #include <typeinfo>
   #include <utility>
   #include <vector>
   
   namespace torch {
   namespace nn {
   
   class AnyModule {
    public:
     class Value;
   
     AnyModule() = default;
   
     template <typename ModuleType>
     explicit AnyModule(std::shared_ptr<ModuleType> module);
   
     template <
         typename ModuleType,
         typename = torch::detail::enable_if_module_t<ModuleType>>
     explicit AnyModule(ModuleType&& module);
   
     template <typename ModuleType>
     explicit AnyModule(const ModuleHolder<ModuleType>& module_holder);
   
     AnyModule(AnyModule&&) = default;
     AnyModule& operator=(AnyModule&&) = default;
   
     AnyModule(const AnyModule& other);
     AnyModule& operator=(const AnyModule& other);
   
     AnyModule clone(at::optional<Device> device = at::nullopt) const;
   
     template <typename ModuleType>
     AnyModule& operator=(std::shared_ptr<ModuleType> module);
   
     template <typename... ArgumentTypes>
     Value forward(ArgumentTypes&&... arguments);
   
     template <typename T, typename = torch::detail::enable_if_module_t<T>>
     T& get();
   
     template <typename T, typename = torch::detail::enable_if_module_t<T>>
     const T& get() const;
   
     template <typename T, typename ContainedType = typename T::ContainedType>
     T get() const;
   
     std::shared_ptr<Module> ptr() const;
   
     template <typename T, typename = torch::detail::enable_if_module_t<T>>
     std::shared_ptr<T> ptr() const;
   
     const std::type_info& type_info() const;
   
     bool is_empty() const noexcept;
   
    private:
     struct Placeholder;
   
     template <typename ModuleType, typename... ArgumentTypes>
     struct Holder;
   
     template <
         typename ModuleType,
         typename Class, // = std::remove_reference<ModuleType>::type
         typename ReturnType,
         typename... ArgumentTypes>
     std::unique_ptr<Placeholder> make_holder(
         std::shared_ptr<ModuleType>&& module,
         ReturnType (Class::*)(ArgumentTypes...));
   
     template <typename T>
     T& get_() const;
   
     std::unique_ptr<Placeholder> content_;
   };
   
   // ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ AnyModule::Value ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
   
   class AnyModule::Value {
    public:
     Value(Value&&) = default;
     Value& operator=(Value&&) = default;
   
     Value(const Value& other) = delete;
     Value& operator=(const Value& other) = delete;
   
     template <typename T>
     T* try_get() {
       static_assert(
           !std::is_reference<T>::value,
           "Value stores decayed types, you cannot cast it to a reference type");
       static_assert(
           !std::is_array<T>::value,
           "Value stores decayed types, you must cast it to T* instead of T[]");
       if (typeid(T).hash_code() == type_info().hash_code()) {
         return &static_cast<Holder<T>&>(*content_).value;
       }
       return nullptr;
     }
   
     template <typename T>
     T get() {
       if (auto* maybe_value = try_get<T>()) {
         return *maybe_value;
       }
       AT_ERROR(
           "Attempted to cast Value to ",
           at::demangle(typeid(T).name()),
           ", but its actual type is ",
           at::demangle(type_info().name()));
     }
   
     const std::type_info& type_info() const noexcept {
       return content_->type_info;
     }
   
    private:
     friend class AnyModule;
     friend struct TestValue;
   
     template <
         typename T,
         typename = torch::disable_if_t<std::is_same<at::Tensor, T>::value>>
     explicit Value(T&& value)
         : content_(
               torch::make_unique<Holder<decay_t<T>>>(std::forward<T>(value))) {}
   
     explicit Value(at::Tensor tensor) {
       if (tensor.is_variable()) {
         content_ = torch::make_unique<Holder<torch::Tensor>>(std::move(tensor));
       } else {
         content_ = torch::make_unique<Holder<at::Tensor>>(std::move(tensor));
       }
     }
   
     struct Placeholder {
       explicit Placeholder(const std::type_info& type_info_) noexcept
           : type_info(type_info_) {}
       virtual ~Placeholder() = default;
       const std::type_info& type_info;
     };
   
     template <typename T>
     struct Holder : public Placeholder {
       template <typename U>
       explicit Holder(U&& value_) noexcept
           : Placeholder(typeid(T)), value(std::forward<U>(value_)) {}
       T value;
     };
   
     std::unique_ptr<Placeholder> content_;
   };
   
   // ~~~~~~~~~~~~~~~~~~~~~~~~~~ AnyModule::Placeholder ~~~~~~~~~~~~~~~~~~~~~~~~~~
   
   struct AnyModule::Placeholder : public AnyModule::Value::Placeholder {
     using AnyModule::Value::Placeholder::Placeholder;
   
     virtual Value forward(std::vector<Value>&& arguments) = 0;
   
     virtual std::shared_ptr<Module> ptr() = 0;
   
     virtual std::unique_ptr<Placeholder> copy() const = 0;
   
     virtual std::unique_ptr<Placeholder> clone(
         at::optional<Device> device) const = 0;
   };
   
   // ~~~~~~~~~~~~~~~~~~~~~~~~~~~~ AnyModule::Holder ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
   
   template <typename ModuleType, typename... ArgumentTypes>
   struct AnyModule::Holder : public AnyModule::Placeholder {
     struct CheckedGetter {
       template <typename T>
       decay_t<T>&& operator()(size_t index) {
         AT_ASSERT(index < arguments_.size());
         auto& value = arguments_[index];
         if (auto* maybe_value = value.template try_get<decay_t<T>>()) {
           return std::move(*maybe_value);
         }
         AT_ERROR(
             "Expected argument #",
             index,
             " to be of type ",
             at::demangle(typeid(T).name()),
             ", but received value of type ",
             at::demangle(value.type_info().name()));
       }
       std::vector<Value>& arguments_;
     };
   
     struct InvokeForward {
       template <typename... Ts>
       Value operator()(Ts&&... ts) {
         return Value(module_->forward(std::forward<Ts>(ts)...));
       }
       std::shared_ptr<ModuleType>& module_;
     };
   
     explicit Holder(std::shared_ptr<ModuleType>&& module_)
         : Placeholder(typeid(ModuleType)), module(std::move(module_)) {}
   
     Value forward(std::vector<Value>&& arguments) override {
       AT_CHECK(
           arguments.size() == sizeof...(ArgumentTypes),
           at::demangle(type_info.name()),
           "'s forward() method expects ",
           sizeof...(ArgumentTypes),
           " arguments, but received ",
           arguments.size());
       // FYI: During invocation of a module's `forward()` method, the values live
       // in the `arguments` vector inside this function.
       return torch::unpack<ArgumentTypes...>(
           InvokeForward{module}, CheckedGetter{arguments});
     }
   
     std::shared_ptr<Module> ptr() override {
       return module;
     }
   
     std::unique_ptr<Placeholder> copy() const override {
       return torch::make_unique<Holder>(*this);
     }
   
     std::unique_ptr<Placeholder> clone(
         at::optional<Device> device) const override {
       return torch::make_unique<Holder>(
           std::static_pointer_cast<ModuleType>(module->clone(device)));
     }
   
     std::shared_ptr<ModuleType> module;
   };
   
   // ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ AnyModule ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
   
   template <typename ModuleType>
   AnyModule::AnyModule(std::shared_ptr<ModuleType> module)
       : content_(make_holder(
             std::move(module),
             &std::remove_reference<ModuleType>::type::forward)) {}
   
   template <typename ModuleType, typename>
   AnyModule::AnyModule(ModuleType&& module)
       : AnyModule(
             std::make_shared<ModuleType>(std::forward<ModuleType>(module))) {}
   
   template <typename ModuleType>
   AnyModule::AnyModule(const ModuleHolder<ModuleType>& module_holder)
       : AnyModule(module_holder.ptr()) {}
   
   inline AnyModule::AnyModule(const AnyModule& other)
       : content_(other.content_ ? other.content_->copy() : nullptr) {}
   
   inline AnyModule& AnyModule::operator=(const AnyModule& other) {
     if (this != &other) {
       content_ = other.content_ ? other.content_->copy() : nullptr;
     }
     return *this;
   }
   
   inline AnyModule AnyModule::clone(at::optional<Device> device) const {
     AnyModule clone;
     clone.content_ = content_ ? content_->clone(device) : nullptr;
     return clone;
   }
   
   template <typename ModuleType>
   AnyModule& AnyModule::operator=(std::shared_ptr<ModuleType> module) {
     return (*this = AnyModule(std::move(module)));
   }
   
   template <typename... ArgumentTypes>
   AnyModule::Value AnyModule::forward(ArgumentTypes&&... arguments) {
     AT_CHECK(!is_empty(), "Cannot call forward() on an empty AnyModule");
     std::vector<Value> values;
     values.reserve(sizeof...(ArgumentTypes));
     torch::apply(
         [&values](Value&& value) { values.push_back(std::move(value)); },
         Value(std::forward<ArgumentTypes>(arguments))...);
     return content_->forward(std::move(values));
   }
   
   template <typename T, typename>
   T& AnyModule::get() {
     AT_CHECK(!is_empty(), "Cannot call get() on an empty AnyModule");
     return get_<T>();
   }
   
   template <typename T, typename>
   const T& AnyModule::get() const {
     AT_CHECK(!is_empty(), "Cannot call get() on an empty AnyModule");
     return get_<T>();
   }
   
   template <typename T, typename ContainedType>
   T AnyModule::get() const {
     return T(ptr<ContainedType>());
   }
   
   inline std::shared_ptr<Module> AnyModule::ptr() const {
     AT_CHECK(!is_empty(), "Cannot call ptr() on an empty AnyModule");
     return content_->ptr();
   }
   
   template <typename T, typename>
   std::shared_ptr<T> AnyModule::ptr() const {
     AT_CHECK(!is_empty(), "Cannot call ptr() on an empty AnyModule");
     get_<T>();
     return std::static_pointer_cast<T>(ptr());
   }
   
   inline const std::type_info& AnyModule::type_info() const {
     AT_CHECK(!is_empty(), "Cannot call type_info() on an empty AnyModule");
     return content_->type_info;
   }
   
   inline bool AnyModule::is_empty() const noexcept {
     return content_ == nullptr;
   }
   
   // Private Methods
   
   template <
       typename ModuleType,
       typename Class,
       typename ReturnType,
       typename... ArgumentTypes>
   std::unique_ptr<AnyModule::Placeholder> AnyModule::make_holder(
       std::shared_ptr<ModuleType>&& module,
       ReturnType (Class::*)(ArgumentTypes...)) {
     static_assert(
         torch::detail::check_not_lvalue_references<ArgumentTypes...>(),
         "Modules stored inside AnyModule must not take references. "
         "Use pointers instead.");
     static_assert(
         !std::is_void<ReturnType>::value,
         "AnyModule cannot store modules that return void "
         "(you can return a dummy value).");
     return torch::make_unique<Holder<decay_t<ModuleType>, ArgumentTypes...>>(
         std::move(module));
   }
   
   template <typename T>
   T& AnyModule::get_() const {
     if (typeid(T).hash_code() == type_info().hash_code()) {
       return *static_cast<Holder<T>&>(*content_).module;
     }
     AT_ERROR(
         "Attempted to cast module of type ",
         at::demangle(type_info().name()),
         " to type ",
         at::demangle(typeid(T).name()));
   }
   
   } // namespace nn
   } // namespace torch
