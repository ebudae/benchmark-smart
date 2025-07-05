# C++ Smart Pointer Performance Benchmark

This repository contains a benchmark to compare the performance overhead of different C++ smart pointer types: raw pointers, `std::unique_ptr`, and `std::shared_ptr`.

Understanding these overheads is crucial for writing high-performance C++ applications, especially in contexts like audio processing where memory management and object lifetime can significantly impact real-time performance.

## Benchmark Details

The benchmark measures the time taken to create, use, and destroy 100,000,000 instances of a simple `MyObject` class using:

1.  **Raw Pointers (`new`/`delete`)**: The baseline for direct memory management.
2.  **`std::unique_ptr`**: For exclusive ownership.
3.  **`std::shared_ptr` (Creation/Destruction)**: For shared ownership, measuring the cost of its reference counting mechanism.
4.  **`std::shared_ptr` (Copying Overhead)**: Specifically measures the cost of copying a `std::shared_ptr`, which involves atomic reference count increments.

## How to Run

You will need a C++ compiler like `g++` or `clang++`.

Navigate to the `smart_pointer_benchmark_repo` directory:

```bash
cd smart_pointer_benchmark_repo
```

Then, compile and run the benchmark:

```bash
# To compile and run without optimizations
g++ -O0 -std=c++17 smart_pointer_benchmark.cpp -o smart_pointer_benchmark_no_opt && ./smart_pointer_benchmark_no_opt

# To compile and run with full optimizations (recommended)
g++ -O3 -std=c++17 smart_pointer_benchmark.cpp -o smart_pointer_benchmark_opt && ./smart_pointer_benchmark_opt
```

## Benchmark Results (Example)

Results from a typical run with `-O3` optimization:

| Test | Time with Optimization (-O3) |
| :--- | :--- |
| 1. Raw Pointers (`new`/`delete`) | ~0.041 s |
| 2. `std::unique_ptr` | ~0.028 s |
| 3. `std::shared_ptr` (Creation/Destruction) | ~1.154 s |
| 4. `std::shared_ptr` (Copying Overhead) | ~0.319 s |

## Key Takeaways

-   **`std::unique_ptr` is a zero-cost abstraction**: It provides automatic memory management with virtually no runtime overhead compared to raw pointers.
-   **`std::shared_ptr` has a significant cost**: Its reference counting mechanism, especially the atomic operations for thread safety, introduces measurable overhead. Use it judiciously only when true shared ownership is required.
-   **Prefer `std::make_unique` and `std::make_shared`**: These factory functions are generally more efficient and exception-safe than direct `new` calls.
