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

1. Retrieve the tiny shakespeare dataset (1.1MB)

```bash
wget -c https://raw.githubusercontent.com/karpathy/char-rnn/master/data/tinyshakespeare/input.txt
```

2. Train the GPT-2 model from the scratch

```bash
python train_gpt2.py
```

In case that you are using the `uv` tool to manage Python dependencies, run the command

```bash
uv run train_gpt2.py
```
