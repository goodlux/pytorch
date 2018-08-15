
.. _program_listing_file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_src_optim_adagrad.cpp:

Program Listing for File adagrad.cpp
====================================

- Return to documentation for :ref:`file__Users_robkunkle_fork_goodlux_pytorch_torch_csrc_api_src_optim_adagrad.cpp`

.. code-block:: cpp

   #include <torch/optim/adagrad.h>
   
   #include <torch/csrc/autograd/variable.h>
   
   #include <ATen/ATen.h>
   
   #include <functional>
   
   namespace torch {
   namespace optim {
   
   AdagradOptions::AdagradOptions(double learning_rate)
       : learning_rate_(learning_rate) {}
   
   const AdagradOptions& Adagrad::options() const noexcept {
     return options_;
   }
   
   void Adagrad::step() {
     for (size_t i = 0; i < parameters_.size(); ++i) {
       auto& grad = parameters_.at(i).grad();
       auto& p = parameters_.at(i).data();
       if (!grad.defined())
         continue;
   
       auto d_p = Tensor(grad).data();
       if (options_.weight_decay_ > 0) {
         d_p.add_(p, options_.weight_decay_);
       }
       step_.at(i) += 1.0;
       auto clr = options_.learning_rate_ /
           (1.0 + (step_.at(i) - 1.0) * options_.lr_decay_);
   
       auto sum = buffer_at(sum_, i);
       sum.data().addcmul_(d_p, d_p, 1.0);
       auto std = sum_.at(i).data().sqrt().add_(1e-10);
       p.addcdiv_(d_p, std, -clr);
     }
   }
   } // namespace optim
   } // namespace torch
