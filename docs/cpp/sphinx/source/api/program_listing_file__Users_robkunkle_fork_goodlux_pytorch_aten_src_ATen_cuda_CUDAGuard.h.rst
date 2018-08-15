
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_CUDAGuard.h:

Program Listing for File CUDAGuard.h
====================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_CUDAGuard.h`

.. code-block:: cpp

   #pragma once
   
   #include <ATen/ArrayRef.h>
   #include <ATen/CUDAStream.h>
   #include <ATen/cuda/CUDAContext.h>
   #include <ATen/DeviceGuard.h>
   
   #include <cstddef>
   #include <vector>
   
   namespace at { namespace cuda {
   
   struct CUDAGuard {
     CUDAGuard() = default;
   
     explicit CUDAGuard(const CUDAStream& stream) {
       set_stream(stream);
     }
   
     explicit CUDAGuard(int32_t device) {
       set_device(device);
     }
   
     CUDAGuard(const CUDAGuard&) = delete;
     CUDAGuard& operator=(const CUDAGuard&) = delete;
   
     CUDAGuard(CUDAGuard&& other) noexcept = default;
   
     CUDAGuard& operator=(CUDAGuard&& other) {
       device_guard_ = std::move(other.device_guard_);
       original_streams_ = std::move(other.original_streams_);
       other.original_streams_.clear();
       return *this;
     }
   
     ~CUDAGuard() {
       if (!original_streams_.empty()) {
         for (size_t device = 0; device < original_streams_.size(); ++device) {
           uncheckedSetCurrentCUDAStreamOnDevice(device, original_streams_[device]);
         }
       }
     }
   
     void set_stream(const CUDAStream& stream) {
       device_guard_.set_index(stream.device());
       // If we haven't stored the current stream yet, store it now.
       if (original_streams_.empty()) {
         const size_t device_count = getNumGPUs();
         original_streams_.reserve(device_count);
         for (size_t device = 0; device < device_count; ++device) {
           original_streams_.push_back(getCurrentCUDAStreamOnDevice(device));
         }
       }
       setCurrentCUDAStreamOnDevice(device_guard_.last_index(), stream);
     }
   
     void set_device(int32_t device) {
       device_guard_.set_index(device);
     }
   
     ArrayRef<CUDAStream> original_streams() const noexcept {
       return original_streams_;
     }
   
     int32_t original_device() const noexcept {
       return device_guard_.original_index();
     }
   
     int32_t last_device() const noexcept {
       return device_guard_.last_index();
     }
   
    private:
     at::DeviceGuard device_guard_;
     std::vector<CUDAStream> original_streams_;
   };
   
   } // namespace cuda
   } // namespace at
