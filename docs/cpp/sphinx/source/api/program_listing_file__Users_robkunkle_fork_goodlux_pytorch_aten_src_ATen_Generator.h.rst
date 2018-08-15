
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Generator.h:

Program Listing for File Generator.h
====================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Generator.h`

.. code-block:: cpp

   #pragma once
   
   #include <stdint.h>
   
   namespace at {
   
   struct Generator {
     Generator() {};
     Generator(const Generator& other) = delete;
     Generator(Generator&& other) = delete;
     virtual ~Generator() {};
   
     virtual Generator& copy(const Generator& other) = 0;
     virtual Generator& free() = 0;
   
     virtual uint64_t seed() = 0;
     virtual uint64_t initialSeed() = 0;
     virtual Generator& manualSeed(uint64_t seed) = 0;
     virtual Generator& manualSeedAll(uint64_t seed) = 0;
     virtual void * unsafeGetTH() = 0;
   };
   
   } // namespace at
