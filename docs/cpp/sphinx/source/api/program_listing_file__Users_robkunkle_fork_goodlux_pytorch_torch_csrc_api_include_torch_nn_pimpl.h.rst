
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_pimpl.h:

Program Listing for File pimpl.h
================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_pimpl.h`

.. code-block:: cpp

   #pragma once
   
   #include <torch/csrc/utils/variadic.h>
   #include <torch/tensor.h>
   
   #include <memory>
   #include <type_traits>
   #include <utility>
   
   namespace torch {
   namespace detail {
   struct ModuleHolderIndicator {};
   
   template <typename T>
   using is_module_holder = std::is_base_of<ModuleHolderIndicator, decay_t<T>>;
   
   template <typename T>
   using disable_if_module_holder_t = disable_if_t<is_module_holder<T>::value>;
   } // namespace detail
   
   namespace nn {
   
   template <typename Contained>
   class ModuleHolder : torch::detail::ModuleHolderIndicator {
    protected:
     std::shared_ptr<Contained> impl_;
   
    public:
     using ContainedType = Contained;
   
     ModuleHolder() : impl_(default_construct()) {
       static_assert(
           std::is_default_constructible<Contained>::value,
           "You are trying to default construct a module which has "
           "no default constructor. Use = nullptr to give it the empty state "
           "(e.g. `Linear linear = nullptr;` instead of `Linear linear;`).");
     }
   
     /* implicit */ ModuleHolder(std::nullptr_t) : impl_(nullptr) {}
   
     template <typename... Ts>
     explicit ModuleHolder(Ts&&... ts)
         : impl_(new Contained(std::forward<Ts>(ts)...)) {}
   
     /* implicit */ ModuleHolder(std::shared_ptr<Contained> module)
         : impl_(std::move(module)) {}
   
     explicit operator bool() const noexcept {
       return !is_empty();
     }
   
     Contained* operator->() {
       return get();
     }
   
     const Contained* operator->() const {
       return get();
     }
   
     Contained& operator*() {
       return *get();
     }
   
     const Contained& operator*() const {
       return *get();
     }
   
     const std::shared_ptr<Contained>& ptr() const {
       AT_CHECK(!is_empty(), "Accessing empty ModuleHolder");
       return impl_;
     }
   
     Contained* get() {
       AT_CHECK(!is_empty(), "Accessing empty ModuleHolder");
       return impl_.get();
     }
   
     const Contained* get() const {
       AT_CHECK(!is_empty(), "Accessing empty ModuleHolder");
       return impl_.get();
     }
   
     template <typename... Args>
     auto operator()(Args&&... args)
         -> decltype((*impl_)(std::forward<Args>(args)...)) {
       return (*impl_)(std::forward<Args>(args)...);
     }
   
     template <typename Arg>
     auto operator[](Arg&& arg) -> decltype((*impl_)[std::forward<Arg>(arg)]) {
       return (*impl_)[std::forward<Arg>(arg)];
     }
   
     bool is_empty() const noexcept {
       return impl_ == nullptr;
     }
   
    private:
   
     template <
         typename T = Contained,
         typename = torch::enable_if_t<std::is_default_constructible<T>::value>>
     std::shared_ptr<Contained> default_construct() {
       return std::make_shared<Contained>();
     }
   
     template <typename T = Contained>
     torch::disable_if_t<
         std::is_default_constructible<T>::value,
         std::shared_ptr<Contained>>
     default_construct() {
       return nullptr;
     }
   };
   } // namespace nn
   } // namespace torch
   
   #define TORCH_ARG(T, name)                          \
     auto name(const T& new_##name)->decltype(*this) { \
       this->name##_ = new_##name;                     \
       return *this;                                   \
     }                                                 \
     auto name(T&& new_##name)->decltype(*this) {      \
       this->name##_ = std::move(new_##name);          \
       return *this;                                   \
     }                                                 \
     const T& name() const noexcept {                  \
       return this->name##_;                           \
     }                                                 \
     T name##_
   
   #define TORCH_MODULE_IMPL(Name, Impl)                            \
     class Name : public torch::nn::ModuleHolder<Impl> {            \
      public:                                                       \
       using torch::nn::ModuleHolder<Impl>::ModuleHolder;           \
       Name(const Name&) = default;                                 \
       Name(Name&&) = default;                                      \
       Name(Name& other) : Name(static_cast<const Name&>(other)) {} \
       Name& operator=(const Name&) = default;                      \
       Name& operator=(Name&&) = default;                           \
     }
   
   #define TORCH_MODULE(Name) TORCH_MODULE_IMPL(Name, Name##Impl)
