This file is used to record the chapter tags that correspond to the latest development process.

Note that these chapter tags are the same as the ones in the original tutorial. You can find these chapters in the description panel of [the page](https://youtu.be/l8pRSuU81PU)

- **v0.1.0**: sample, auto-detect the device
- **v0.2.0**: optimization loop: overfit a single batch
- **v0.3.0**: data loader lite
- **v0.4.0**: model initialization: std 0.02, residual init
- **v0.5.0 (current version)**: Tensor Cores, timing the code, TF32 precision, 333ms
  - Benchmark results on my server (NVIDIA L40 × 4):

    | dtype   | dt (ms) | tok/sec  |
    |:-------:|--------:|---------:|
    | float32 | 904.37  | 18514.44 |
