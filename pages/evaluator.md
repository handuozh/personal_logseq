---
title: Evaluator
---

- Evaluater 提供了最小二乘函数const function的求解，参数的更新等接口
- Constructor
    -
      ```C++
      Options {
      	int num_eliminate_blocks = -1;
        LinearSolverType linear_solver_type = DENSE_QR;
        bool dynamic_sparsity = false;
        ContextImpl* context = nullptr;
        EvaluationCallback* evaluation_callback = nullptr;
      }
      ```
    -
    -
      ```C++
      Evaluator* Evaluator::Create(const Evaluator::Option& options,
      Program* program, std::string* error){
      }
      ```
    -