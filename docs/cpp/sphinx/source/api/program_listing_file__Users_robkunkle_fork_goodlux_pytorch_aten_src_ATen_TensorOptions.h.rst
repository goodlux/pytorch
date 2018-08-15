
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_TensorOptions.h:

Program Listing for File TensorOptions.h
========================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_TensorOptions.h`

.. code-block:: cpp

   #pragma once
   
   #include <ATen/Context.h>
   #include <ATen/Device.h>
   #include <ATen/DeviceGuard.h>
   #include <ATen/Layout.h>
   #include <ATen/ScalarType.h>
   #include <ATen/Tensor.h>
   #include <ATen/Type.h>
   
   #include <cstddef>
   #include <iosfwd>
   #include <utility>
   
   namespace at {
   
   struct AT_API TensorOptions {
     TensorOptions() : TensorOptions(/*use_thread_local_default_options=*/true) {}
   
     explicit TensorOptions(bool use_thread_local_default_options);
   
     explicit TensorOptions(Tensor tensor, bool discard_runtime_type = false) {
       if (!discard_runtime_type) {
         type_ = &tensor.type();
       }
       this->dtype(tensor.dtype());
       this->device(tensor.device());
       this->layout(tensor.layout());
     }
   
     /* implicit */ TensorOptions(
         const Type& type,
         int32_t device_index = -1,
         bool discard_runtime_type = false) {
       if (!discard_runtime_type) {
         type_ = &type;
       }
       this->dtype(type.scalarType());
       this->device({type.backend(), device_index});
       this->layout(type.layout());
     }
   
     /* implicit */ TensorOptions(Layout layout) : TensorOptions() {
       this->layout(layout);
     }
   
     /* implicit */ TensorOptions(Device device) : TensorOptions() {
       this->device(device);
     }
   
     /* implicit */ TensorOptions(Backend backend)
         : TensorOptions(Device(backend)) {}
   
     /* implicit */ TensorOptions(ScalarType dtype) : TensorOptions() {
       this->dtype(dtype);
     }
   
     bool operator==(const TensorOptions& other) const noexcept {
       return dtype_ == other.dtype_ && layout_ == other.layout_ &&
           device_ == other.device_ && requires_grad_ == other.requires_grad_;
     }
   
     bool operator!=(const TensorOptions& other) const noexcept {
       return !(*this == other);
     }
   
     const TensorOptions& discard_runtime_type() const {
       type_ = nullptr;
       return *this;
     }
   
     TensorOptions& device(Device device) {
       device_ = std::move(device);
       update_underlying_type();
       return *this;
     }
   
     TensorOptions& device_index(int32_t device_index) {
       return device({Device::Type::CUDA, device_index});
     }
   
     TensorOptions& dtype(ScalarType dtype) {
       dtype_ = dtype;
       update_underlying_type();
       return *this;
     }
   
     TensorOptions& layout(Layout layout) {
       layout_ = layout;
       update_underlying_type();
       return *this;
     }
   
     TensorOptions& requires_grad(bool requires_grad) {
       requires_grad_ = requires_grad;
       return *this;
     }
   
     const Device& device() const noexcept {
       return device_;
     }
   
     int32_t device_index() const noexcept {
       return device_.index();
     }
   
     ScalarType dtype() const noexcept {
       return dtype_;
     }
   
     Layout layout() const noexcept {
       return layout_;
     }
   
     bool requires_grad() const noexcept {
       return requires_grad_;
     }
   
     const Type& type() const {
       if (type_ != nullptr) {
         return *type_;
       }
       return getType(backend(), dtype_);
     }
   
    private:
     void update_underlying_type() {
       if (type_) {
         type_ = &type_->toScalarType(dtype_).toBackend(backend());
       }
     }
   
     // Resolves the ATen backend specified by the current construction axes.
     Backend backend() const noexcept {
       Backend backend;
       if (device_.type() == Device::Type::CPU) {
         backend = (layout_ == kStrided) ? kCPU : kSparseCPU;
       } else {
         backend = (layout_ == kStrided) ? kCUDA : kSparseCUDA;
       }
       return backend;
     }
   
    private:
     ScalarType dtype_{kFloat};
     Device device_{Device::Type::CPU};
     Layout layout_{Layout::Strided};
     bool requires_grad_{false};
     // Not part of the observable API, so make `mutable` so we can set it to
     // `null` in `discard_runtime_type`.
     mutable const Type* type_{nullptr};
   };
   
   inline TensorOptions dtype(ScalarType dtype) {
     return TensorOptions().dtype(dtype);
   }
   
   inline TensorOptions layout(Layout layout) {
     return TensorOptions().layout(layout);
   }
   
   inline TensorOptions device(Device device) {
     return TensorOptions().device(std::move(device));
   }
   
   inline TensorOptions device_index(int32_t device_index) {
     return TensorOptions().device_index(device_index);
   }
   
   inline TensorOptions requires_grad(bool requires_grad = true) {
     return TensorOptions().requires_grad(requires_grad);
   }
   
   inline TensorOptions Tensor::options() const {
     return TensorOptions(*this);
   }
   
   namespace detail {
   inline Tensor to(
       const Tensor& tensor,
       const TensorOptions& options,
       bool non_blocking) {
     // Don't copy if the options match.
     if (tensor.options() == options) {
       return tensor;
     }
     DeviceGuard guard(options.device());
     return options.type().copy(tensor, non_blocking);
   }
   } // namespace detail
   
   inline Tensor Tensor::to(Device device, ScalarType dtype, bool non_blocking)
       const {
     if (this->device() == device && this->dtype() == dtype) {
       return *this;
     }
     return detail::to(*this, options().device(device).dtype(dtype), non_blocking);
   }
   
   inline Tensor Tensor::to(ScalarType dtype, bool non_blocking) const {
     if (this->dtype() == dtype) {
       return *this;
     }
     return detail::to(*this, options().dtype(dtype), non_blocking);
   }
   
   inline Tensor Tensor::to(Device device, bool non_blocking) const {
     if (this->device() == device) {
       return *this;
     }
     return detail::to(*this, options().device(device), non_blocking);
   }
   } // namespace at
   
   std::ostream& operator<<(
       std::ostream& stream,
       const at::TensorOptions& options);
