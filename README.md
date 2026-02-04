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

```bash
python train_gpt2.py
```

In case that you are using the `uv` tool to manage Python dependencies, run the command

```bash
uv run train_gpt2.py
```
