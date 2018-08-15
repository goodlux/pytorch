
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_SparseTensorRef.h:

Program Listing for File SparseTensorRef.h
==========================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_SparseTensorRef.h`

.. code-block:: cpp

   #pragma once
   
   namespace at {
   
   struct Tensor;
   struct SparseTensorRef {
     explicit SparseTensorRef(const Tensor& t): tref(t) {}
     const Tensor& tref;
   };
   
   }
