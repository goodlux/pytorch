

.. _file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_cuda_CuFFTPlanCache.h:

File CuFFTPlanCache.h
=====================




.. contents:: Contents
   :local:
   :backlinks: none

Definition (``/Users/robkunkle/fork/goodlux/pytorch/aten/src/ATen/native/cuda/CuFFTPlanCache.h``)
-------------------------------------------------------------------------------------------------


.. toctree::
   :maxdepth: 1

   program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_cuda_CuFFTPlanCache.h.rst





Includes
--------


- ``ATen/ATen.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_ATen.h`)

- ``ATen/Config.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Config.h`)

- ``ATen/cuda/CUDAContext.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_CUDAContext.h`)

- ``ATen/native/cuda/CuFFTUtils.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_cuda_CuFFTUtils.h`)

- ``ATen/native/utils/ParamsHash.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_utils_ParamsHash.h`)

- ``cufft.h``

- ``cufftXt.h``

- ``limits``

- ``list`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_tensor_list_view.h`)

- ``sstream``

- ``stdexcept``

- ``string``

- ``unordered_map``



Included By
-----------


- :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_detail_CUDAHooks.cpp`




Namespaces
----------


- :ref:`namespace_at`

- :ref:`namespace_at__native`

- :ref:`namespace_at__native__detail`


Classes
-------


- :ref:`struct_at__native__detail__CuFFTHandleDeleter`

- :ref:`struct_at__native__detail__CuFFTParams`

- :ref:`class_at__native__detail__CuFFTConfig`

- :ref:`class_at__native__detail__CuFFTParamsLRUCache`


Functions
---------


- :ref:`function_at__native__detail__cufft_clear_plan_cache_impl`

- :ref:`function_at__native__detail__cufft_get_plan_cache_max_size_impl`

- :ref:`function_at__native__detail__cufft_get_plan_cache_size_impl`

- :ref:`function_at__native__detail__cufft_set_plan_cache_max_size_impl`

- :ref:`function_at__native__detail__is_pow_of_two`

- :ref:`function_at__native__detail__setCuFFTParams`


Variables
---------


- :ref:`variable_at__native__detail__CUFFT_MAX_PLAN_NUM`

