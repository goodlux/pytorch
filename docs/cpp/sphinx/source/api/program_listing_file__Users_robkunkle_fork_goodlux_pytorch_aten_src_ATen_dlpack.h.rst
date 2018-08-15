
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_dlpack.h:

Program Listing for File dlpack.h
=================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_dlpack.h`

.. code-block:: cpp

   
   #ifndef DLPACK_DLPACK_H_
   #define DLPACK_DLPACK_H_
   
   #ifdef __cplusplus
   #define DLPACK_EXTERN_C extern "C"
   #else
   #define DLPACK_EXTERN_C
   #endif
   
   #define DLPACK_VERSION 010
   
   #ifdef _WIN32
   #ifdef DLPACK_EXPORTS
   #define DLPACK_DLL __declspec(dllexport)
   #else
   #define DLPACK_DLL __declspec(dllimport)
   #endif
   #else
   #define DLPACK_DLL
   #endif
   
   #include <stdint.h>
   #include <stddef.h>
   
   #ifdef __cplusplus
   extern "C" {
   #endif
   
   typedef enum {
     kDLCPU = 1,
     kDLGPU = 2,
     // kDLCPUPinned = kDLCPU | kDLGPU
     kDLCPUPinned = 3,
     kDLOpenCL = 4,
     kDLMetal = 8,
     kDLVPI = 9,
     kDLROCM = 10,
   } DLDeviceType;
   
   typedef struct {
     DLDeviceType device_type;
     int device_id;
   } DLContext;
   
   typedef enum {
     kDLInt = 0U,
     kDLUInt = 1U,
     kDLFloat = 2U,
   } DLDataTypeCode;
   
   typedef struct {
     uint8_t code;
     uint8_t bits;
     uint16_t lanes;
   } DLDataType;
   
   typedef struct {
     void* data;
     DLContext ctx;
     int ndim;
     DLDataType dtype;
     int64_t* shape;
     int64_t* strides;
     uint64_t byte_offset;
   } DLTensor;
   
   typedef struct DLManagedTensor {
     DLTensor dl_tensor;
     void * manager_ctx;
     void (*deleter)(struct DLManagedTensor * self);
   } DLManagedTensor;
   #ifdef __cplusplus
   }  // DLPACK_EXTERN_C
   #endif
   #endif  // DLPACK_DLPACK_H_
