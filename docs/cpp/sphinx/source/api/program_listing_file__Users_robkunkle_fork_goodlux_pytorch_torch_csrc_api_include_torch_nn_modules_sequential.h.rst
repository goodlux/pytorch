
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_modules_sequential.h:

Program Listing for File sequential.h
=====================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_modules_sequential.h`

.. code-block:: cpp

   #pragma once
   
   #include <torch/detail/static.h>
   #include <torch/nn/module.h>
   #include <torch/nn/modules/any.h>
   #include <torch/nn/pimpl.h>
   #include <torch/tensor.h>
   
   #include <ATen/Error.h>
   
   #include <cstdint>
   #include <memory>
   #include <string>
   #include <type_traits>
   #include <utility>
   #include <vector>
   
   namespace torch {
   namespace nn {
   class SequentialImpl : public Cloneable<SequentialImpl> {
    public:
     using Iterator = std::vector<AnyModule>::iterator;
     using ConstIterator = std::vector<AnyModule>::const_iterator;
   
     template <typename... Modules>
     explicit SequentialImpl(Modules&&... modules) {
       modules_.reserve(sizeof...(Modules));
       push_back(std::forward<Modules>(modules)...);
     }
   
     std::shared_ptr<Module> clone(
         at::optional<Device> device = at::nullopt) const override {
       auto clone = std::make_shared<SequentialImpl>();
       for (const auto& module : modules_) {
         clone->push_back(module.clone(device));
       }
       return clone;
     }
   
     void reset() override {}
   
     template <typename ReturnType = Tensor, typename... ArgumentTypes>
     ReturnType forward(ArgumentTypes&&... arguments) {
       AT_CHECK(!is_empty(), "Cannot call forward() on an empty Sequential");
   
       auto iterator = modules_.begin();
       auto input = iterator->forward(std::forward<ArgumentTypes>(arguments)...);
   
       for (++iterator; iterator != modules_.end(); ++iterator) {
         input = iterator->forward(std::move(input));
       }
   
       // Check the return value and give a nice error message if the requsted
       // return type was incorrect.
       if (auto* return_value = input.template try_get<ReturnType>()) {
         return std::move(*return_value);
       }
       AT_ERROR(
           "The type of the return value is ",
           at::demangle(input.type_info().name()),
           ", but you asked for type ",
           at::demangle(typeid(ReturnType).name()));
     }
   
     template <typename ModuleType>
     void push_back(std::shared_ptr<ModuleType> module_ptr) {
       // Nesting Sequential doesn't work because `forward()`'s return type is
       // templatized, so it'll give a nasty compiler error.
       static_assert(
           !std::is_same<SequentialImpl, ModuleType>::value,
           "Sequential is not nestable");
       static_assert(
           torch::detail::is_module<ModuleType>::value,
           "Can only add objects derived from nn::Module to Sequential");
       static_assert(
           torch::detail::has_forward<ModuleType>::value,
           "Can only add modules with a forward() method to Sequential");
       push_back(AnyModule(std::move(module_ptr)));
     }
   
     template <typename M, typename = torch::detail::enable_if_module_t<M>>
     void push_back(M&& module) {
       // Need to get rid of any reference components for make_unique.
       using Type = typename std::remove_reference<M>::type;
       // Here we move (or copy) the module into a new shared_ptr.
       push_back(std::make_shared<Type>(std::forward<M>(module)));
     }
   
     template <typename M>
     void push_back(const ModuleHolder<M>& module_holder) {
       push_back(module_holder.ptr());
     }
   
     template <typename Container>
     void extend(const Container& container) {
       for (const auto& module : container) {
         push_back(module);
       }
     }
   
     Iterator begin() {
       return modules_.begin();
     }
     ConstIterator begin() const {
       return modules_.begin();
     }
   
     Iterator end() {
       return modules_.end();
     }
     ConstIterator end() const {
       return modules_.end();
     }
   
     template <typename T>
     T& at(size_t index) {
       static_assert(
           torch::detail::is_module<T>::value,
           "Can only call Sequential::at with an nn::Module type");
       AT_CHECK(index < size(), "Index out of range");
       return modules_[index].get<T>();
     }
   
     template <typename T>
     const T& at(size_t index) const {
       static_assert(
           torch::detail::is_module<T>::value,
           "Can only call Sequential::at with an nn::Module type");
       AT_CHECK(index < size(), "Index out of range");
       return modules_[index].get<T>();
     }
   
     std::shared_ptr<Module> ptr(size_t index) const {
       AT_CHECK(index < size(), "Index out of range");
       return modules_[index].ptr();
     }
   
     template <typename T>
     std::shared_ptr<T> ptr(size_t index) const {
       static_assert(
           torch::detail::is_module<T>::value,
           "Can only call Sequential::ptr with an nn::Module type");
       AT_CHECK(index < size(), "Index out of range");
       return modules_[index].ptr<T>();
     }
   
     std::shared_ptr<Module> operator[](size_t index) const {
       // This is the only method we can call without a type.
       return ptr(index);
     }
   
     size_t size() const noexcept {
       return modules_.size();
     }
   
     bool is_empty() const noexcept {
       return size() == 0;
     }
   
    private:
     template <typename First, typename Second, typename... Rest>
     void push_back(First&& first, Second&& second, Rest&&... rest) {
       push_back(std::forward<First>(first));
       // Recursively calls this method, until the parameter pack only thas this
       // entry left. Then calls `push_back()` a final time (above).
       push_back(std::forward<Second>(second), std::forward<Rest>(rest)...);
     }
   
     void push_back(AnyModule any_module) {
       modules_.push_back(std::move(any_module));
       const auto index = modules_.size() - 1;
       register_module(std::to_string(index), modules_[index].ptr());
     }
   
     void push_back() {}
   
     // Box the AnyModules to give Sequential reference semantics, like the rest of
     // the API. Note that this is not required otherwise, this could just be a
     // `vector<AnyModule>`.
     std::vector<AnyModule> modules_;
   };
   
   TORCH_MODULE(Sequential);
   } // namespace nn
   } // namespace torch
