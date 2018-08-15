
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_MatrixRef.h:

Program Listing for File MatrixRef.h
====================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_MatrixRef.h`

.. code-block:: cpp

   #pragma once
   #include <ATen/ArrayRef.h>
   #include <ATen/Utils.h>
   
   #include <vector>
   
   namespace at {
     template<typename T>
     class MatrixRef {
     public:
       typedef size_t size_type;
   
     private:
       ArrayRef<T> arr;
   
       size_type stride0;
   
       // Stride of dim 1 is assumed to be 1
   
     public:
       /*implicit*/ MatrixRef() : arr(nullptr), stride0(0) {}
   
       /*implicit*/ MatrixRef(ArrayRef<T> arr, size_type stride0)
         : arr(arr), stride0(stride0) {
           AT_CHECK(arr.size() % stride0 == 0, "MatrixRef: ArrayRef size ", arr.size(), " not divisible by stride ", stride0)
         }
   
   
       bool empty() const { return arr.empty(); }
   
       const T *data() const { return arr.data(); }
   
       size_t size(size_t dim) const {
         if (dim == 0) {
           return arr.size() / stride0;
         } else if (dim == 1) {
           return stride0;
         } else {
           AT_CHECK(0, "MatrixRef: out of bounds dimension ", dim, "; expected 0 or 1");
         }
       }
   
       size_t numel() const {
         return arr.size();
       }
   
       bool equals(MatrixRef RHS) const {
         return stride0 == RHS.stride0 && arr.equals(RHS.arr);
       }
   
       ArrayRef<T> operator[](size_t Index) const {
         return arr.slice(Index*stride0, stride0);
       }
   
       template <typename U>
       typename std::enable_if<std::is_same<U, T>::value, MatrixRef<T>>::type &
       operator=(U &&Temporary) = delete;
   
       template <typename U>
       typename std::enable_if<std::is_same<U, T>::value, MatrixRef<T>>::type &
       operator=(std::initializer_list<U>) = delete;
   
     };
   
   } // end namespace at
