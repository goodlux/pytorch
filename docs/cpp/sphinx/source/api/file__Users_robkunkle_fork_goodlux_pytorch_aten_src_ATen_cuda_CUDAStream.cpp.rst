

.. _file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_CUDAStream.cpp:

File CUDAStream.cpp
===================




.. contents:: Contents
   :local:
   :backlinks: none

Definition (``/Users/robkunkle/fork/goodlux/pytorch/aten/src/ATen/cuda/CUDAStream.cpp``)
----------------------------------------------------------------------------------------


.. toctree::
   :maxdepth: 1

   program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_CUDAStream.cpp.rst





Includes
--------


- ``ATen/Error.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Error.h`)

- ``ATen/cuda/CUDAContext.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_CUDAContext.h`)

- ``ATen/cuda/CUDAStream.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_CUDAStream.h`)

- ``ATen/cuda/Exceptions.h`` (:ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cuda_Exceptions.h`)

- ``atomic``

- ``mutex``






Namespaces
----------


- :ref:`namespace_at`

- :ref:`namespace_at__cuda`

- :ref:`namespace_at__cuda__detail`


Classes
-------


- :ref:`struct_CUDAStreamInternals`


Functions
---------


- :ref:`function_at__cuda__detail__check_gpu`

- :ref:`function_at__cuda__detail__CUDAStream_createAndRetainWithOptions`

- :ref:`function_at__cuda__detail__CUDAStream_device`

- :ref:`function_at__cuda__detail__CUDAStream_free`

- :ref:`function_at__cuda__detail__CUDAStream_getAndRetainCurrentStream`

- :ref:`function_at__cuda__detail__CUDAStream_getAndRetainCurrentStreamOnDevice`

- :ref:`function_at__cuda__detail__CUDAStream_getCurrentStreamOnDeviceUnsafe`

- :ref:`function_at__cuda__detail__CUDAStream_getCurrentStreamUnsafe`

- :ref:`function_at__cuda__detail__CUDAStream_getDefaultStream`

- :ref:`function_at__cuda__detail__CUDAStream_getDefaultStreamOnDevice`

- :ref:`function_at__cuda__detail__CUDAStream_retain`

- :ref:`function_at__cuda__detail__CUDAStream_setStream`

- :ref:`function_at__cuda__detail__CUDAStream_setStreamOnDevice`

- :ref:`function_at__cuda__detail__CUDAStream_stream`

- :ref:`function_at__cuda__detail__CUDAStream_uncheckedFree`

- :ref:`function_at__cuda__detail__CUDAStream_uncheckedSetStreamOnDevice`

- :ref:`function_at__cuda__detail__initCUDAStreamsOnce`

- :ref:`function_at__cuda__detail__initDefaultCUDAStreams`


Variables
---------


- :ref:`variable_at__cuda__detail__current_streams`

- :ref:`variable_at__cuda__detail__DEFAULT_STREAM`

- :ref:`variable_at__cuda__detail__default_streams`

- :ref:`variable_at__cuda__detail__init_flag`

- :ref:`variable_at__cuda__detail__num_gpus`

