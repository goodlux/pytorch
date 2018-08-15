
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_AlignOf.h:

Program Listing for File AlignOf.h
==================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_AlignOf.h`

.. code-block:: cpp

   //===--- AlignOf.h - Portable calculation of type alignment -----*- C++ -*-===//
   //
   //                     The LLVM Compiler Infrastructure
   //
   // This file is distributed under the University of Illinois Open Source
   // License. See LICENSE.TXT for details.
   //
   //===----------------------------------------------------------------------===//
   //
   // This file defines the AlignedCharArray and AlignedCharArrayUnion classes.
   //
   //===----------------------------------------------------------------------===//
   
   // ATen: modified from llvm::AlignOf
   // replaced LLVM_ALIGNAS with alignas
   
   #pragma once
   
   #include <cstddef>
   
   namespace at {
   
   
   // MSVC requires special handling here.
   #ifndef _MSC_VER
   
   template<size_t Alignment, size_t Size>
   struct AlignedCharArray {
     alignas(Alignment) char buffer[Size];
   };
   
   #else // _MSC_VER
   
   template<size_t Alignment, size_t Size>
   struct AlignedCharArray;
   
   // We provide special variations of this template for the most common
   // alignments because __declspec(align(...)) doesn't actually work when it is
   // a member of a by-value function argument in MSVC, even if the alignment
   // request is something reasonably like 8-byte or 16-byte. Note that we can't
   // even include the declspec with the union that forces the alignment because
   // MSVC warns on the existence of the declspec despite the union member forcing
   // proper alignment.
   
   template<size_t Size>
   struct AlignedCharArray<1, Size> {
     union {
       char aligned;
       char buffer[Size];
     };
   };
   
   template<size_t Size>
   struct AlignedCharArray<2, Size> {
     union {
       short aligned;
       char buffer[Size];
     };
   };
   
   template<size_t Size>
   struct AlignedCharArray<4, Size> {
     union {
       int aligned;
       char buffer[Size];
     };
   };
   
   template<size_t Size>
   struct AlignedCharArray<8, Size> {
     union {
       double aligned;
       char buffer[Size];
     };
   };
   
   
   // The rest of these are provided with a __declspec(align(...)) and we simply
   // can't pass them by-value as function arguments on MSVC.
   
   #define AT_ALIGNEDCHARARRAY_TEMPLATE_ALIGNMENT(x) \
     template<size_t Size> \
     struct AlignedCharArray<x, Size> { \
       __declspec(align(x)) char buffer[Size]; \
     };
   
   AT_ALIGNEDCHARARRAY_TEMPLATE_ALIGNMENT(16)
   AT_ALIGNEDCHARARRAY_TEMPLATE_ALIGNMENT(32)
   AT_ALIGNEDCHARARRAY_TEMPLATE_ALIGNMENT(64)
   AT_ALIGNEDCHARARRAY_TEMPLATE_ALIGNMENT(128)
   
   #undef AT_ALIGNEDCHARARRAY_TEMPLATE_ALIGNMENT
   
   #endif // _MSC_VER
   
   namespace detail {
   template <typename T1,
             typename T2 = char, typename T3 = char, typename T4 = char,
             typename T5 = char, typename T6 = char, typename T7 = char,
             typename T8 = char, typename T9 = char, typename T10 = char>
   class AlignerImpl {
     T1 t1; T2 t2; T3 t3; T4 t4; T5 t5; T6 t6; T7 t7; T8 t8; T9 t9; T10 t10;
   
     AlignerImpl() = delete;
   };
   
   template <typename T1,
             typename T2 = char, typename T3 = char, typename T4 = char,
             typename T5 = char, typename T6 = char, typename T7 = char,
             typename T8 = char, typename T9 = char, typename T10 = char>
   union SizerImpl {
     char arr1[sizeof(T1)], arr2[sizeof(T2)], arr3[sizeof(T3)], arr4[sizeof(T4)],
          arr5[sizeof(T5)], arr6[sizeof(T6)], arr7[sizeof(T7)], arr8[sizeof(T8)],
          arr9[sizeof(T9)], arr10[sizeof(T10)];
   };
   } // end namespace detail
   
   template <typename T1,
             typename T2 = char, typename T3 = char, typename T4 = char,
             typename T5 = char, typename T6 = char, typename T7 = char,
             typename T8 = char, typename T9 = char, typename T10 = char>
   struct AlignedCharArrayUnion : AlignedCharArray<
       alignof(detail::AlignerImpl<T1, T2, T3, T4, T5,
                                         T6, T7, T8, T9, T10>),
       sizeof(::at::detail::SizerImpl<T1, T2, T3, T4, T5,
                                        T6, T7, T8, T9, T10>)> {
   };
   } // end namespace at
