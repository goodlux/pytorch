
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Device.h:

Program Listing for File Device.h
=================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Device.h`

.. code-block:: cpp

   #pragma once
   
   #include <ATen/Error.h>
   #include <ATen/ScalarType.h>
   
   #include <cstddef>
   #include <iosfwd>
   #include <string>
   #include <functional>
   
   namespace at {
   struct Device {
     enum class Type { CPU, CUDA };
   
     static Type backend_to_type(Backend backend) {
       switch (backend) {
         case kCPU:
         case kSparseCPU:
           return Type::CPU;
         case kCUDA:
         case kSparseCUDA:
           return Type::CUDA;
         default:
           AT_ERROR(
               "Invalid backend ", toString(backend), " for Device construction");
       }
     }
   
     /* implicit */ Device(Type type, int32_t index = -1)
         : type_(type), index_(index) {
       AT_CHECK(
           index == -1 || index >= 0,
           "Device index must be -1 or non-negative, got ",
           index);
       AT_CHECK(
           !is_cpu() || index <= 0,
           "CPU device index must be -1 or zero, got ",
           index);
     }
   
     /* implicit */ Device(const std::string& device_string);
   
     /* implicit */ Device(Backend backend, int32_t index = -1)
         : Device(backend_to_type(backend), index) {}
   
     bool operator==(const Device& other) const noexcept {
       return this->type_ == other.type_ && this->index_ == other.index_;
     }
   
     bool operator!=(const Device& other) const noexcept {
       return !(*this == other);
     }
   
     void set_index(int32_t index) {
       index_ = index;
     }
   
     Type type() const noexcept {
       return type_;
     }
   
     const int32_t& index() const noexcept {
       return index_;
     }
   
     bool has_index() const noexcept {
       return index_ != -1;
     }
   
     bool is_cuda() const noexcept {
       return type_ == Type::CUDA;
     }
   
     bool is_cpu() const noexcept {
       return type_ == Type::CPU;
     }
   
    private:
     Type type_;
     int32_t index_ = -1;
   };
   } // namespace at
   
   AT_API std::ostream& operator<<(std::ostream& stream, at::Device::Type type);
   AT_API std::ostream& operator<<(std::ostream& stream, const at::Device& device);
   
   namespace std {
     template<> struct hash<at::Device>
     {
       size_t operator()(const at::Device& device) const noexcept {
         size_t hash_val = static_cast<size_t>(device.index() + 1);
         if (device.is_cuda()) {
           hash_val += 2;
         }
         return hash_val;
       }
     };
   } // namespace std
