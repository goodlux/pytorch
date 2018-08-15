
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_SmallVector.cpp:

Program Listing for File SmallVector.cpp
========================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_SmallVector.cpp`

.. code-block:: cpp

   //===- llvm/ADT/SmallVector.cpp - 'Normally small' vectors ----------------===//
   //
   //                     The LLVM Compiler Infrastructure
   //
   // This file is distributed under the University of Illinois Open Source
   // License. See LICENSE.TXT for details.
   //
   //===----------------------------------------------------------------------===//
   //
   // This file implements the SmallVector class.
   //
   //===----------------------------------------------------------------------===//
   
   // ATen: modified from llvm::SmallVector.
   // replaced report_bad_alloc_error with std::bad_alloc
   
   #include "SmallVector.h"
   
   namespace at {
   
   void SmallVectorBase::grow_pod(void *FirstEl, size_t MinSizeInBytes,
                                  size_t TSize) {
     size_t CurSizeBytes = size_in_bytes();
     size_t NewCapacityInBytes = 2 * capacity_in_bytes() + TSize; // Always grow.
     if (NewCapacityInBytes < MinSizeInBytes)
       NewCapacityInBytes = MinSizeInBytes;
   
     void *NewElts;
     if (BeginX == FirstEl) {
       NewElts = malloc(NewCapacityInBytes);
       if (NewElts == nullptr)
         throw std::bad_alloc();
   
       // Copy the elements over.  No need to run dtors on PODs.
       memcpy(NewElts, this->BeginX, CurSizeBytes);
     } else {
       // If this wasn't grown from the inline copy, grow the allocated space.
       NewElts = realloc(this->BeginX, NewCapacityInBytes);
       if (NewElts == nullptr)
         throw std::bad_alloc();
     }
   
     this->EndX = (char*)NewElts+CurSizeBytes;
     this->BeginX = NewElts;
     this->CapacityX = (char*)this->BeginX + NewCapacityInBytes;
   }
   
   }
