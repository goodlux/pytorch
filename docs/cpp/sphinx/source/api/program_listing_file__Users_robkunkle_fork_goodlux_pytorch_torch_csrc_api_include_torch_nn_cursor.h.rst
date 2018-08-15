
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_cursor.h:

Program Listing for File cursor.h
=================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_include_torch_nn_cursor.h`

.. code-block:: cpp

   #pragma once
   
   #include <torch/tensor.h>
   
   #include <cstddef>
   #include <iterator>
   #include <limits>
   #include <string>
   #include <type_traits>
   
   // Forward declarations.
   namespace torch {
   namespace detail {
   template <typename T>
   struct CursorCollector;
   } // namespace detail
   namespace nn {
   class Module;
   } // namespace nn
   } // namespace torch
   
   namespace torch {
   namespace detail {
   template <typename T>
   class CursorBase {
    public:
     using ValueType = T;
   
     // NOTE: This is a template class, but we explicitly instantiate it in the
     // .cpp file for every type necessary, so we can define it in the .cpp file.
     // Hooray!
   
     struct Item {
       Item(const std::string& key_, T& module_);
   
       T& operator*();
       const T& operator*() const;
       T* operator->();
       const T* operator->() const;
   
       const std::string key;
       T& value;
     };
   
     // Iterators are valid for the lifetime of the cursor.
   
     // Picks either `const_iterator` or `iterator` as the iterator type, depending
     // on whether `T` is const.
     using Iterator = typename std::vector<Item>::iterator;
     using ConstIterator = typename std::vector<Item>::const_iterator;
   
     CursorBase() = default;
   
     explicit CursorBase(std::vector<Item>&& items);
   
     // No need for a virtual destructor, as cursors are not intended to be used
     // polymorhpically (i.e. we are relying on non-virtual inheritance).
   
     // Note that these functions may only be called on lvalues (that's the
     // ampersand next to the function)! This prevents code like `auto iterator =
     // module.modules().begin()`, since `iterator` would be pointing to a `vector`
     // that gets destructed at the end of the expression. This is not a problem
     // for range loops, as they capture the range expression (the thing to the
     // right of the colon in `for (auto x : ...)`) before iteration. This is
     // smart.
     Iterator begin() & noexcept;
     ConstIterator begin() const& noexcept;
   
     Iterator end() & noexcept;
     ConstIterator end() const& noexcept;
   
     template <typename Function>
     void apply(const Function& function) {
       for (auto& item : items_) {
         function(*item);
       }
     }
     template <typename Function>
     void apply(const Function& function) const {
       for (auto& item : items_) {
         function(*item);
       }
     }
   
     template <typename Function>
     void apply_items(const Function& function) {
       for (auto& item : items_) {
         function(item.key, item.value);
       }
     }
     template <typename Function>
     void apply_items(const Function& function) const {
       for (auto& item : items_) {
         function(item.key, item.value);
       }
     }
   
     template <typename Iterator, typename Function>
     void map(Iterator output_iterator, Function function) {
       for (auto& item : items_) {
         *output_iterator++ = function(*item);
       }
     }
     template <typename Iterator, typename Function>
     void map(Iterator output_iterator, Function function) const {
       for (auto& item : items_) {
         *output_iterator++ = function(*item);
       }
     }
   
     template <typename Iterator, typename Function>
     void map_items(Iterator output_iterator, Function function) {
       for (auto& item : items_) {
         *output_iterator++ = function(item.key, item.value);
       }
     }
     template <typename Iterator, typename Function>
     void map_items(Iterator output_iterator, Function function) const {
       for (auto& item : items_) {
         *output_iterator++ = function(item.key, item.value);
       }
     }
   
     T* find(const std::string& key) noexcept;
     const T* find(const std::string& key) const noexcept;
   
     T& at(const std::string& key);
     const T& at(const std::string& key) const;
   
     Item& at(size_t index);
   
     T& operator[](const std::string& key);
     const T& operator[](const std::string& key) const;
   
     Item& operator[](size_t index);
   
     bool contains(const std::string& key) const noexcept;
   
     size_t size() const noexcept;
   
    protected:
     struct Collector;
   
     std::vector<Item> items_;
   };
   } // namespace detail
   
   namespace nn {
   
   // Module cursors (`.modules()` and `.children()`)
   
   class ModuleCursor : public detail::CursorBase<Module> {
    public:
     friend class ConstModuleCursor;
     explicit ModuleCursor(
         Module& module,
         size_t maximum_depth = std::numeric_limits<size_t>::max());
   };
   
   class ConstModuleCursor : public detail::CursorBase<const Module> {
    public:
     explicit ConstModuleCursor(
         const Module& module,
         size_t maximum_depth = std::numeric_limits<size_t>::max());
   
     /* implicit */ ConstModuleCursor(const ModuleCursor& cursor);
   };
   
   // Parameter cursors (`.parameters()`)
   
   class ParameterCursor : public detail::CursorBase<Tensor> {
    public:
     friend class ConstParameterCursor;
     explicit ParameterCursor(Module& module);
   };
   
   class ConstParameterCursor : public detail::CursorBase<const Tensor> {
    public:
     explicit ConstParameterCursor(const Module& module);
     /* implicit */ ConstParameterCursor(const ParameterCursor& cursor);
   };
   
   // Buffer cursors (`.buffers()`)
   
   class BufferCursor : public detail::CursorBase<Tensor> {
    public:
     friend class ConstBufferCursor;
     explicit BufferCursor(Module& module);
   };
   
   class ConstBufferCursor : public detail::CursorBase<const Tensor> {
    public:
     explicit ConstBufferCursor(const Module& module);
     /* implicit */ ConstBufferCursor(const BufferCursor& cursor);
   };
   } // namespace nn
   } // namespace torch
