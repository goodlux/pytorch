
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Error.h:

Program Listing for File Error.h
================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_aten_src_ATen_Error.h`

.. code-block:: cpp

   #pragma once
   
   #include <ATen/ATenGeneral.h> // for AT_API
   #include <ATen/optional.h>
   
   #include <cstddef>
   #include <exception>
   #include <ostream>
   #include <sstream>
   #include <string>
   
   #if defined(_MSC_VER) && _MSC_VER <= 1900
   #define __func__ __FUNCTION__
   #endif
   
   namespace at {
   
   namespace detail {
   
   inline std::ostream& _str(std::ostream& ss) { return ss; }
   
   template <typename T>
   inline std::ostream& _str(std::ostream& ss, const T& t) {
     ss << t;
     return ss;
   }
   
   template <typename T, typename... Args>
   inline std::ostream&
   _str(std::ostream& ss, const T& t, const Args&... args) {
     return _str(_str(ss, t), args...);
   }
   
   } // namespace detail
   
   // Convert a list of string-like arguments into a single string.
   template <typename... Args>
   inline std::string str(const Args&... args) {
     std::ostringstream ss;
     detail::_str(ss, args...);
     return ss.str();
   }
   
   // Specializations for already-a-string types.
   template <>
   inline std::string str(const std::string& str) {
     return str;
   }
   inline std::string str(const char* c_str) {
     return c_str;
   }
   
   struct SourceLocation {
     const char* function;
     const char* file;
     uint32_t line;
   };
   
   std::ostream& operator<<(std::ostream& out, const SourceLocation& loc);
   
   class AT_API Error : public std::exception {
     std::string what_without_backtrace_;
     std::string what_;
   
   public:
     Error(SourceLocation source_location, std::string err);
   
     const char* what() const noexcept override {
       return what_.c_str();
     }
   
     const char* what_without_backtrace() const noexcept {
       return what_without_backtrace_.c_str();
     }
   };
   
   class AT_API Warning {
     using handler_t = void(*)(const SourceLocation& source_location, const char* msg);
   
   public:
     static void warn(SourceLocation source_location, std::string msg);
   
     static void set_warning_handler(handler_t handler);
   
     static void print_warning(const SourceLocation& source_location, const char* msg);
   
   private:
     static handler_t warning_handler_;
   };
   
   
   } // namespace at
   
   // TODO: variants that print the expression tested and thus don't require strings
   // TODO: CAFFE_ENFORCE_WITH_CALLER style macro
   
   #define AT_ERROR(...) \
     throw at::Error({__func__, __FILE__, __LINE__}, at::str(__VA_ARGS__))
   
   #define AT_WARN(...) \
     at::Warning::warn({__func__, __FILE__, __LINE__}, at::str(__VA_ARGS__))
   
   #define AT_ASSERT(cond) \
     if (!(cond)) {             \
       AT_ERROR(#cond " ASSERT FAILED at ", __FILE__, ":", __LINE__, ", please report a bug to PyTorch.");   \
     }
   
   #define AT_ASSERTM(cond, ...) \
     if (!(cond)) {             \
       AT_ERROR(at::str(#cond, " ASSERT FAILED at ", __FILE__, ":", __LINE__, ", please report a bug to PyTorch. ", __VA_ARGS__));   \
     }
   
   #define AT_CHECK(cond, ...) \
     if (!(cond)) {             \
       AT_ERROR(at::str(__VA_ARGS__));   \
     }
