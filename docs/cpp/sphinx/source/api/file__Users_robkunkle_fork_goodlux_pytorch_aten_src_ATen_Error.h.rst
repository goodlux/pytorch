

.. _file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Error.h:

File Error.h
============




.. contents:: Contents
   :local:
   :backlinks: none

Definition (``/Users/robkunkle/fork/goodlux/pytorch/aten/src/ATen/Error.h``)
----------------------------------------------------------------------------


.. toctree::
   :maxdepth: 1

   program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Error.h.rst





Includes
--------


- ``ATen/ATenGeneral.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_ATenGeneral.h`)

- ``ATen/optional.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_optional.h`)

- ``cstddef``

- ``exception``

- ``ostream``

- ``sstream``

- ``string``



Included By
-----------


- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_detail_ordered_dict.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_expanding_array.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_cloneable.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_modules_rnn.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_modules_sequential.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_parallel_data_parallel.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_src_nn_init.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_src_nn_module.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_src_nn_modules_batchnorm.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_src_nn_modules_dropout.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_src_nn_modules_rnn.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Allocator.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_ArrayRef.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_CheckGenerator.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Context.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_CPUFixedAllocator.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_CUDAStream.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_detail_CUDAHooks.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_Exceptions.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_detail_CUDAHooksInterface.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_detail_CUDAHooksInterface.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_detail_VariableHooksInterface.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Device.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Device.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_DeviceGuard.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Dispatch.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Error.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_ExpandUtils.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Layout.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_cudnn_RNN.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_DispatchStub.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_DispatchStub.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_Distributions.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_Memory.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_Pooling.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_TensorCompare.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_TensorFactories.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_TensorShape.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_TensorTransformations.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_TensorTransformations.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_SparseTensorImpl.h`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_UndefinedTensor.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_UndefinedType.cpp`

- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Utils.h`




Namespaces
----------


- :ref:`namespace_at`

- :ref:`namespace_at__detail`


Classes
-------


- :ref:`struct_at__SourceLocation`

- :ref:`class_at__Error`

- :ref:`class_at__Warning`


Functions
---------


- :ref:`function_at__detail___str`

- :ref:`function_at__detail___str`

- :ref:`function_at__detail___str`

- :ref:`function_at__str`

- :ref:`function_at__str`

- :ref:`function_at__str`


Defines
-------


- :ref:`define_AT_ASSERT`

- :ref:`define_AT_ASSERTM`

- :ref:`define_AT_CHECK`

- :ref:`define_AT_ERROR`

- :ref:`define_AT_WARN`

