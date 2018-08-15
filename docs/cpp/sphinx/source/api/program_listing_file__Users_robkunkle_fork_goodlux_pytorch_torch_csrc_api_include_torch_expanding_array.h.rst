
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_expanding_array.h:

Program Listing for File expanding_array.h
==========================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_expanding_array.h`

.. code-block:: cpp

   #pragma once
   
   #include <ATen/ArrayRef.h>
   #include <ATen/Error.h>
   
   #include <algorithm>
   #include <array>
   #include <cstdint>
   #include <initializer_list>
   #include <vector>
   
   namespace torch {
   
   template <size_t D, typename T = int64_t>
   class ExpandingArray {
    public:
     /*implicit*/ ExpandingArray(std::initializer_list<T> list)
         : ExpandingArray(std::vector<T>(list)) {}
   
     /*implicit*/ ExpandingArray(const std::vector<T>& values) {
       AT_CHECK(
           values.size() == D,
           "Expected ",
           D,
           " values, but instead got ",
           values.size());
       std::copy(values.begin(), values.end(), values_.begin());
     }
   
     /*implicit*/ ExpandingArray(T single_size) {
       values_.fill(single_size);
     }
   
     /*implicit*/ ExpandingArray(const std::array<T, D>& values)
         : values_(values) {}
   
     std::array<T, D>& operator*() {
       return values_;
     }
   
     const std::array<T, D>& operator*() const {
       return values_;
     }
   
     std::array<T, D>* operator->() {
       return &values_;
     }
   
     const std::array<T, D>* operator->() const {
       return &values_;
     }
   
     operator at::ArrayRef<T>() const {
       return values_;
     }
   
     size_t size() const noexcept {
       return D;
     }
   
    private:
     std::array<T, D> values_;
   };
   
   } // namespace torch
