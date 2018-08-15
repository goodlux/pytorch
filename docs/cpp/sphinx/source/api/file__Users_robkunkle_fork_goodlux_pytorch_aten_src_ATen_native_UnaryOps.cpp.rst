

.. _file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_UnaryOps.cpp:

File UnaryOps.cpp
=================




.. contents:: Contents
   :local:
   :backlinks: none

Definition (``/Users/robkunkle/fork/goodlux/pytorch/aten/src/ATen/native/UnaryOps.cpp``)
----------------------------------------------------------------------------------------


.. toctree::
   :maxdepth: 1

   program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_UnaryOps.cpp.rst





Includes
--------


- ``ATen/ATen.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_ATen.h`)

- ``ATen/CPUApplyUtils.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_CPUApplyUtils.h`)

- ``ATen/Dispatch.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Dispatch.h`)

- ``ATen/ExpandUtils.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_ExpandUtils.h`)

- ``ATen/NativeFunctions.h``

- ``ATen/Parallel.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Parallel.h`)

- ``ATen/WrapDimUtils.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_WrapDimUtils.h`)

- ``ATen/native/cpu/UnaryOpsKernel.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_native_cpu_UnaryOpsKernel.h`)

- ``algorithm``

- ``cmath``

- ``functional`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_src_nn_modules_functional.cpp`)

- ``map``

- ``numeric``

- ``vector``






Namespaces
----------


- :ref:`namespace_at`

- :ref:`namespace_at__native`


Functions
---------


- :ref:`function_at__native___abs__cpu`

- :ref:`function_at__native___abs_out_cpu`

- :ref:`function_at__native___acos__cpu`

- :ref:`function_at__native___acos_out_cpu`

- :ref:`function_at__native___asin__cpu`

- :ref:`function_at__native___asin_out_cpu`

- :ref:`function_at__native___atan__cpu`

- :ref:`function_at__native___atan_out_cpu`

- :ref:`function_at__native___ceil__cpu`

- :ref:`function_at__native___ceil_out_cpu`

- :ref:`function_at__native___clamp__cpu`

- :ref:`function_at__native___clamp_max__cpu`

- :ref:`function_at__native___clamp_max_out_cpu`

- :ref:`function_at__native___clamp_min__cpu`

- :ref:`function_at__native___clamp_min_out_cpu`

- :ref:`function_at__native___clamp_out_cpu`

- :ref:`function_at__native___cos__cpu`

- :ref:`function_at__native___cos_out_cpu`

- :ref:`function_at__native___cosh__cpu`

- :ref:`function_at__native___cosh_out_cpu`

- :ref:`function_at__native___erf__cpu`

- :ref:`function_at__native___erf_out_cpu`

- :ref:`function_at__native___erfc__cpu`

- :ref:`function_at__native___erfc_out_cpu`

- :ref:`function_at__native___exp__cpu`

- :ref:`function_at__native___exp_out_cpu`

- :ref:`function_at__native___expm1__cpu`

- :ref:`function_at__native___expm1_out_cpu`

- :ref:`function_at__native___floor__cpu`

- :ref:`function_at__native___floor_out_cpu`

- :ref:`function_at__native___log10__cpu`

- :ref:`function_at__native___log10_out_cpu`

- :ref:`function_at__native___log1p__cpu`

- :ref:`function_at__native___log1p_out_cpu`

- :ref:`function_at__native___log2__cpu`

- :ref:`function_at__native___log2_out_cpu`

- :ref:`function_at__native___log__cpu`

- :ref:`function_at__native___log_out_cpu`

- :ref:`function_at__native___round__cpu`

- :ref:`function_at__native___round_out_cpu`

- :ref:`function_at__native___rsqrt__cpu`

- :ref:`function_at__native___rsqrt_out_cpu`

- :ref:`function_at__native___sigmoid__cpu`

- :ref:`function_at__native___sigmoid_out_cpu`

- :ref:`function_at__native___sin__cpu`

- :ref:`function_at__native___sin_out_cpu`

- :ref:`function_at__native___sinh__cpu`

- :ref:`function_at__native___sinh_out_cpu`

- :ref:`function_at__native___sqrt__cpu`

- :ref:`function_at__native___sqrt_out_cpu`

- :ref:`function_at__native___tan__cpu`

- :ref:`function_at__native___tan_out_cpu`

- :ref:`function_at__native___tanh__cpu`

- :ref:`function_at__native___tanh_out_cpu`

- :ref:`function_at__native___trunc__cpu`

- :ref:`function_at__native___trunc_out_cpu`

- :ref:`function_at__native__abs`

- :ref:`function_at__native__acos`

- :ref:`function_at__native__asin`

- :ref:`function_at__native__atan`

- :ref:`function_at__native__ceil`

- :ref:`function_at__native__clamp`

- :ref:`function_at__native__clamp_max`

- :ref:`function_at__native__clamp_min`

- :ref:`function_at__native__cos`

- :ref:`function_at__native__cosh`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__DEFINE_DISPATCH`

- :ref:`function_at__native__erf`

- :ref:`function_at__native__erfc`

- :ref:`function_at__native__exp`

- :ref:`function_at__native__expm1`

- :ref:`function_at__native__fill`

- :ref:`function_at__native__fill`

- :ref:`function_at__native__floor`

- :ref:`function_at__native__log`

- :ref:`function_at__native__log10`

- :ref:`function_at__native__log1p`

- :ref:`function_at__native__log2`

- :ref:`function_at__native__round`

- :ref:`function_at__native__rsqrt`

- :ref:`function_at__native__sigmoid`

- :ref:`function_at__native__sin`

- :ref:`function_at__native__sinh`

- :ref:`function_at__native__sqrt`

- :ref:`function_at__native__tan`

- :ref:`function_at__native__tanh`

- :ref:`function_at__native__trunc`


Defines
-------


- :ref:`define_IMPLEMENT_UNARY_OP_TH`

- :ref:`define_IMPLEMENT_UNARY_OP_VEC`

