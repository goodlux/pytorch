
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_templates_TensorDense.cpp:

Program Listing for File TensorDense.cpp
========================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_templates_TensorDense.cpp`

.. code-block:: cpp

   // included as 'TensorDenseOrSparse' in TensorDerived.cpp
   
   IntList ${Tensor}::strides() const {
     // NB: THTensor doesn't agree with Tensor for scalars, so we
     // have to construct a fresh IntList
     return IntList(THTensor_getStridePtr(tensor), dim());
   }
   Scalar ${Tensor}::localScalar() {
     int64_t numel = ${THTensor}_nElement(${state,}tensor);
     AT_CHECK(numel == 1,"a Tensor with ", numel, " elements cannot be converted to Scalar");
     return Scalar(${to_at_type}(${THStorage}_get(${state,} THTensor_getStoragePtr(tensor), tensor->storage_offset())));
   }
   std::unique_ptr<Storage> ${Tensor}::storage() {
     auto storage = ${THTensor}_storage(${state,}tensor);
     ${THStorage}_retain(${state,}storage);
     return std::unique_ptr<Storage>(new ${Storage}(&type().get_context(), storage));
   }
