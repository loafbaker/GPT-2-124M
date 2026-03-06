# GPT-2-124M

The project is to reproduce a simplified version of GPT-2 (124M) from scratch, following [Andrej Kapathy's tutorial](https://youtu.be/l8pRSuU81PU).

## Guide to prepare the virtual environment

The code has been tested on a multi-GPU server (NVIDIA L40 × 4).

The virtual environment can be prepared with `pip`.

```bash
virtualenv venv
source venv/bin/activate

python -m pip install -r requirements.txt
```

Alternatively, the virtual environment can be prepared with `uv`.

```bash
uv sync
```

## Guide to run the code

### 1. Prepare for the training data

#### 1.1 Download and tokenize the Fineweb-EDU dataset for training

> **Note:** This instruction works with **version ≥ 0.14.0**.

Run the following script to download and tokenize the [FineWeb-EDU dataset](https://huggingface.co/datasets/HuggingFaceFW/fineweb-edu):

```bash
python fineweb.py
```

After the script finishes, the processed dataset shards will be stored in:

```
./edu_fineweb10B
```

The total size of the generated data files is approximately **19GB**.

#### 1.2 Download the tiny shakespeare dataset

> **Note:** This instruction works with **version ≤ 0.13.0**.

You can download the Tiny Shakespeare dataset with:

```bash
wget -c https://raw.githubusercontent.com/karpathy/char-rnn/master/data/tinyshakespeare/input.txt
```

The data file is pretty small, with a size of **~1.1MB**.

### 2. Train the GPT-2 model from the scratch

> **Note:** The `micro batch size` parameter (`B`) can be tuned according to the performance of your current system. In my script, it is set to `32` based on the specifications of my GPU server.

#### 2.1 Single-GPU training

```bash
python train_gpt2.py
```

In case that you are using the `uv` tool to manage Python dependencies, run the command

```bash
uv run train_gpt2.py
```

#### 2.2 Multi-GPU training with Distributed Data Parallel (DDP)

```bash
torchrun --standalone --nproc_per_node=4 train_gpt2.py
```

> **Note:** `--nproc_per_node=4` specifies the number of GPUs to use for training. Adjust this value according to the number of GPUs available on your system.

> **Note:** When using `uv`, prefix the command with `uv run`.

## Error handling

### 1. Triton backend error when compiling a PyTorch model

If you encounter a Triton backend error when compiling the PyTorch model with

```python
model = torch.compile(model)
```

the error message may look like the following:

```
torch._dynamo.exc.BackendCompilerFailed: backend='inductor' raised:
RuntimeError: Cannot find a working triton installation. More information on installing Triton can be found at https://github.com/openai/triton
```

To diagnose the issue, try importing the `triton` library in Python to check whether it is installed correctly:

```python
import triton
```

You may encounter the following error:

```
ModuleNotFoundError: No module named 'setuptools'
```

This indicates that the root cause of the Triton backend problem is a missing Python dependency: `setuptools`.

To resolve this issue, install the missing library using either `pip` or `uv`:

```bash
# option 1: install with pip
pip install setuptools

# option 2: install with uv
uv add setuptools
```

### 2. Triton `libcuda` not found error

You may encounter an error related to a missing CUDA library `libcuda.so`, the error message may look like:

```
torch._dynamo.exc.BackendCompilerFailed: backend='inductor' raised:
AssertionError: libcuda.so cannot found!
Possible files are located at ['/lib/x86_64-linux-gnu/libcuda.so.1'].Please create a symlink of libcuda.so to any of the file.
```

First, verify that the CUDA driver is properly installed by running:

```bash
nvidia-smi
```

If this command fails, you may need to check your CUDA or NVIDIA driver installation.

If `nvidia-smi` works correctly, this is a CUDA library linking issue related to the location of the `libcuda.so` library.

The file is typically located in the `lib64/` directory inside the CUDA installation directory, for example:

```
/usr/local/cuda-12/lib64/stubs
```

Find the directory containing `libcuda.so`, then set the environment variable `TRITON_LIBCUDA_PATH` to that directory:

```bash
export TRITON_LIBCUDA_PATH=/usr/local/cuda-12/lib64/stubs
```

After setting this environment variable, the Triton `libcuda` not found issue should be resolved.
