
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_ArrayRef.h:

Program Listing for File ArrayRef.h
===================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_ArrayRef.h`

.. code-block:: cpp

   //===--- ArrayRef.h - Array Reference Wrapper -------------------*- C++ -*-===//
   //
   //                     The LLVM Compiler Infrastructure
   //
   // This file is distributed under the University of Illinois Open Source
   // License. See LICENSE.TXT for details.
   //
   //===----------------------------------------------------------------------===//
   
   // ATen: modified from llvm::ArrayRef.
   // removed llvm-specific functionality
   // removed some implicit const -> non-const conversions that rely on
   // complicated std::enable_if meta-programming
   // removed a bunch of slice variants for simplicity...
   
   #pragma once
   
   #include <ATen/Error.h>
   #include <ATen/SmallVector.h>
   
   #include <array>
   #include <iterator>
   #include <vector>
   
   namespace at {
     template<typename T>
     class ArrayRef {
     public:
       typedef const T *iterator;
       typedef const T *const_iterator;
       typedef size_t size_type;
   
       typedef std::reverse_iterator<iterator> reverse_iterator;
   
     private:
       const T *Data;
   
       size_type Length;
   
     public:
   
       /*implicit*/ ArrayRef() : Data(nullptr), Length(0) {}
   
       /*implicit*/ ArrayRef(const T &OneElt)
         : Data(&OneElt), Length(1) {}
   
       /*implicit*/ ArrayRef(const T *data, size_t length)
         : Data(data), Length(length) {}
   
       ArrayRef(const T *begin, const T *end)
         : Data(begin), Length(end - begin) {}
   
       template<typename U>
       /*implicit*/ ArrayRef(const SmallVectorTemplateCommon<T, U> &Vec)
         : Data(Vec.data()), Length(Vec.size()) {
       }
   
       template<typename A>
       /*implicit*/ ArrayRef(const std::vector<T, A> &Vec)
         : Data(Vec.data()), Length(Vec.size()) {}
   
       template <size_t N>
       /*implicit*/ constexpr ArrayRef(const std::array<T, N> &Arr)
           : Data(Arr.data()), Length(N) {}
   
       template <size_t N>
       /*implicit*/ constexpr ArrayRef(const T (&Arr)[N]) : Data(Arr), Length(N) {}
   
       /*implicit*/ ArrayRef(const std::initializer_list<T> &Vec)
       : Data(Vec.begin() == Vec.end() ? (T*)nullptr : Vec.begin()),
         Length(Vec.size()) {}
   
   
       const_iterator begin() const { return Data; }
       const_iterator end() const { return Data + Length; }
   
       reverse_iterator rbegin() const { return reverse_iterator(end()); }
       reverse_iterator rend() const { return reverse_iterator(begin()); }
   
       bool empty() const { return Length == 0; }
   
       const T *data() const { return Data; }
   
       size_t size() const { return Length; }
   
       const T &front() const {
         AT_CHECK(!empty(), "ArrayRef: attempted to access front() of empty list");
         return Data[0];
       }
   
       const T &back() const {
         AT_CHECK(!empty(), "ArrayRef: attempted to access back() of empty list");
         return Data[Length-1];
       }
   
       bool equals(ArrayRef RHS) const {
         if (Length != RHS.Length)
           return false;
         return std::equal(begin(), end(), RHS.begin());
       }
   
       ArrayRef<T> slice(size_t N, size_t M) const {
         AT_CHECK(N+M <= size(), "ArrayRef: invalid slice, ", N, " + ", M, " is not <= ", size());
         return ArrayRef<T>(data()+N, M);
       }
   
       ArrayRef<T> slice(size_t N) const { return slice(N, size() - N); }
   
       const T &operator[](size_t Index) const {
         return Data[Index];
       }
   
       const T &at(size_t Index) const {
         AT_CHECK(Index < Length, "ArrayRef: invalid index ", Index, " for length ", Length);
         return Data[Index];
       }
   
       template <typename U>
       typename std::enable_if<std::is_same<U, T>::value, ArrayRef<T>>::type &
       operator=(U &&Temporary) = delete;
   
       template <typename U>
       typename std::enable_if<std::is_same<U, T>::value, ArrayRef<T>>::type &
       operator=(std::initializer_list<U>) = delete;
   
       std::vector<T> vec() const {
         return std::vector<T>(Data, Data+Length);
       }
   
       operator std::vector<T>() const {
         return std::vector<T>(Data, Data+Length);
       }
   
     };
   
   } // end namespace at
