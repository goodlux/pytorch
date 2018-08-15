
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_detail_UniqueVoidPtr.cpp:

Program Listing for File UniqueVoidPtr.cpp
==========================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_detail_UniqueVoidPtr.cpp`

.. code-block:: cpp

   #include <ATen/detail/UniqueVoidPtr.h>
   
   namespace at { namespace detail {
   
   void deleteNothing(void*) {}
   
   }} // namespace at
