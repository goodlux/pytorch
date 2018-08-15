
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_mkl_README.md:

Program Listing for File README.md
==================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_mkl_README.md`

.. code-block:: cpp

   All files living in this directory are written with the assumption that MKL is available,
   which means that these code are not guarded by `#if AT_MKL_ENABLED()`. Therefore, whenever
   you need to use definitions from here, please guard the `#include<ATen/mkl/*.h>` and
   definition usages with `#if AT_MKL_ENABLED()` macro, e.g. [SpectralOps.cpp](native/mkl/SpectralOps.cpp).
