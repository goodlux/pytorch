
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_mkldnn_Runtime.h:

Program Listing for File Runtime.h
==================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_mkldnn_Runtime.h`

.. code-block:: cpp

   #pragma once
   
   #include <mkldnn.hpp>
   
   using namespace mkldnn;
   
   namespace at { namespace native {
   
   // CpuEngine singleton
   struct CpuEngine {
     static CpuEngine& Instance() {
       static CpuEngine myInstance;
       return myInstance;
     }
     engine& get_engine() {
       return _cpu_engine;
     }
     CpuEngine(CpuEngine const&) = delete;
     CpuEngine& operator=(CpuEngine const&) = delete;
   
   protected:
     CpuEngine():_cpu_engine(mkldnn::engine::cpu, 0) {}
     ~CpuEngine() {}
   
   private:
     engine _cpu_engine;
   };
   
   // Stream singleton
   struct Stream {
     static Stream& Instance() {
       static Stream myInstance;
       return myInstance;
     };
     stream& get_stream() {
       return _cpu_stream;
     }
     Stream(Stream const&) = delete;
     Stream& operator=(Stream const&) = delete;
   
   protected:
     Stream():_cpu_stream(mkldnn::stream::kind::eager) {}
     ~Stream() {}
   
   private:
     stream _cpu_stream;
   };
   
   }}  // namespace at::native
