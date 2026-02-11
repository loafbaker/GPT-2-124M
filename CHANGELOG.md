This file is used to record the chapter tags that correspond to the latest development process.

Note that these chapter tags are the same as the ones in the original tutorial. You can find these chapters in the description panel of [the page](https://youtu.be/l8pRSuU81PU)

- **v0.1.0**: sample, auto-detect the device
- **v0.2.0**: optimization loop: overfit a single batch
- **v0.3.0**: data loader lite
- **v0.4.0**: model initialization: std 0.02, residual init
- **v0.5.0 and v0.5.1**: Tensor Cores, timing the code, TF32 precision, 333ms
- **v0.5.2**: float16, gradient scalers, bfloat16, 300ms
- **v0.6.0 (current version)**: torch.compile, Python overhead, kernel fusion, 130ms
  - Benchmark results on my server (NVIDIA L40 × 4):

    | dtype    | torch.compile? | dt (ms) | tok/sec  |
    |:--------:|:--------------:|--------:|---------:|
    | float32  | No             | 904.37  | 18514.44 |
    | tfloat32 | No             | 688.66  | 23887.96 |
    | bfloat16 | No             | 532.77  | 30940.47 |
    | bfloat16 | Yes            | 1058.92 | 56368.43 |
