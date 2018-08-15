
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cudnn_README.md:

Program Listing for File README.md
==================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_cudnn_README.md`

.. code-block:: cpp

   All files living in this directory are written with the assumption that cuDNN is available,
   which means that these code are not guarded by `#if AT_CUDNN_ENABLED()`. Therefore, whenever
   you need to use definitions from here, please guard the `#include<ATen/cudnn/*.h>` and
   definition usages with `#if AT_CUDNN_ENABLED()` macro, e.g. [native/cudnn/BatchNorm.cpp](native/cudnn/BatchNorm.cpp).
