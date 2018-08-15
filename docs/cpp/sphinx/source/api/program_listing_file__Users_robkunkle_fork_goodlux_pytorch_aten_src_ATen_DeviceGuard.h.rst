
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_DeviceGuard.h:

Program Listing for File DeviceGuard.h
======================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_DeviceGuard.h`

.. code-block:: cpp

   #pragma once
   
   #include <ATen/Device.h>
   #include <ATen/Error.h>
   #include <ATen/ScalarType.h>
   #include <ATen/Tensor.h>
   #include <ATen/detail/CUDAHooksInterface.h>
   
   #include <cstddef>
   
   namespace at {
   struct DeviceGuard {
     DeviceGuard() = default;
   
     explicit DeviceGuard(Device device) {
       if (device.is_cuda()) {
         set_index(device.index());
       }
     }
   
     explicit DeviceGuard(int32_t index) {
       set_index(index);
     }
   
     explicit DeviceGuard(const Tensor& tensor) {
       set_index_from(tensor);
     }
   
     explicit DeviceGuard(const TensorList& tensors) {
       if (!tensors.empty()) {
         set_index_from(tensors.front());
       }
     }
   
     DeviceGuard(const DeviceGuard&) = delete;
     DeviceGuard& operator=(const DeviceGuard&) = delete;
   
     DeviceGuard(DeviceGuard&& other) noexcept {
       *this = std::move(other);
     }
   
     DeviceGuard& operator=(DeviceGuard&& other) noexcept {
       this->original_index_ = other.original_index_;
       this->last_index_ = other.last_index_;
       // Set other's original index to the unspecified/default state, so that it
       // doesn't also reset the device in its constructor.
       other.original_index_ = -1;
       return *this;
     }
   
     ~DeviceGuard() {
       // It should only not have a value if an index was never actually set.
       if (original_index_ != -1) {
         // Unchecked because we don't want to throw in the destructor.
         detail::DynamicCUDAInterface::unchecked_set_device(original_index_);
       }
     }
   
     void set_index(int32_t index) {
       if (index == -1) {
         return;
       }
       AT_ASSERT(index >= 0);
       if (original_index_ == -1) {
         int32_t previous_index = -123;
         detail::DynamicCUDAInterface::get_device(&previous_index);
         original_index_ = previous_index;
         if (index != original_index_) {
           detail::DynamicCUDAInterface::set_device(index);
         }
       } else {
         detail::DynamicCUDAInterface::set_device(index);
       }
       last_index_ = index;
     }
   
     void set_index_from(const Tensor& tensor) {
       if (tensor.defined() && tensor.is_cuda()) {
         set_index(tensor.get_device());
       }
     }
   
     int32_t original_index() const noexcept {
       return original_index_;
     }
   
     int32_t last_index() const noexcept {
       return last_index_;
     }
   
    private:
     int32_t original_index_ = -1;
     int32_t last_index_ = -1;
   };
   } // namespace at
