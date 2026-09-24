# Experiment 002 — Local Model Benchmark Baseline

## Objective

Evaluate three locally hosted Ollama models on the same Windows development machine using controlled prompts and a deterministic coding validation task.

Models evaluated:

- `qwen3-vl-2b-4k`
- `qwen2.5-coder:3b`
- `llama3.2:latest`

This experiment is a baseline characterization of model behavior on the project hardware. It is not an overall ranking of model quality.

## Environment

- Runtime: Ollama
- Execution: local CPU inference
- Platform: Windows
- Benchmark location: `personal-ai-infrastructure`
- Text benchmark: no image input
- Models were evaluated individually rather than intentionally benchmarking simultaneous model loading.

## Test 01 — Technical Explanation Latency

Prompt:

> Explain DNS, TCP, and HTTP. Give exactly one sentence for each. Do not use bullet points.

Measured end-to-end wall-clock latency:

| Model | Time |
|---|---:|
| `qwen3-vl-2b-4k` | 15.0813924 s |
| `qwen2.5-coder:3b` | 17.3703015 s |
| `llama3.2:latest` | 79.5527728 s |

### Interpretation

These measurements represent end-to-end CLI latency measured with PowerShell `Measure-Command`. They should not be interpreted as pure token-generation speed or as a general model performance ranking.

The experiment only establishes observed latency for this prompt, runtime configuration, and machine.

## Test 02 — Strict Output Contract

Prompt:

> Return exactly three lines. Line 1 must contain DNS. Line 2 must contain TCP. Line 3 must contain HTTP. Do not add any other text.

### Results

| Model | Semantic requirement | Exact format |
|---|---|---|
| `qwen3-vl-2b-4k` | Understood task | Failed |
| `qwen2.5-coder:3b` | Understood task | Failed |
| `llama3.2:latest` | Understood task | Failed |

`qwen3-vl-2b-4k` produced an extended reasoning trace instead of immediately returning the required three-line artifact.

`qwen2.5-coder:3b` and `llama3.2:latest` produced the requested DNS/TCP/HTTP semantic content, but did not satisfy the literal output contract exactly.

### Finding

Semantic compliance and literal output-format compliance are separate evaluation dimensions.

## Test 03 — Coding Task

Prompt:

> Write a Python function called `is_valid_ipv4` that takes a string and returns True if it is a valid IPv4 address and False otherwise. Do not use external libraries. Return only the Python code.

### Qwen3-VL

The model entered an extended reasoning process and did not produce a completed function in the captured output.

Result:

- Complete implementation: No
- Code-only contract: No

### Qwen2.5-Coder

The model produced a usable `is_valid_ipv4` implementation covering:

- four dot-separated parts
- numeric parts
- values from 0 through 255

However, it also added explanatory material instead of returning only Python code.

Result:

- Complete implementation: Yes
- Basic functionality: Yes
- Code-only contract: No

### Llama 3.2

The model produced a usable `is_valid_ipv4` implementation covering:

- four dot-separated parts
- numeric parts
- values from 0 through 255
- rejection of multi-character octets beginning with `0`

However, it returned the code in a Markdown code block rather than strictly returning only Python code.

Result:

- Complete implementation: Yes
- Basic functionality: Yes
- Code-only contract: No

## Test 03A — Deterministic Code Validation

For this benchmark, the following explicit rule was used:

> IPv4 octets containing leading zeros are treated as invalid.

The generated implementation under test was evaluated against 11 deterministic cases:

| Input | Expected |
|---|---|
| `192.168.1.1` | True |
| `8.8.8.8` | True |
| `0.0.0.0` | True |
| `255.255.255.255` | True |
| `256.1.1.1` | False |
| `192.168.1` | False |
| `192.168.1.1.1` | False |
| `192.168.-1.1` | False |
| `192.168.a.1` | False |
| `` | False |
| `192.168.001.1` | False |

Validation result:

**11/11 tests passed.**

## Engineering Interpretation

This experiment demonstrates three distinct evaluation dimensions:

1. **Latency** — how long the end-to-end request took.
2. **Artifact correctness** — whether generated code actually works against deterministic tests.
3. **Instruction compliance** — whether the model followed the requested output contract.

A model can succeed on one dimension and fail another.

The benchmark therefore avoids reducing model evaluation to a single score.

## Limitations

- The benchmark uses a small number of prompts.
- Latency measurements are end-to-end wall-clock measurements.
- Only one coding problem was validated.
- The results are specific to this hardware, runtime, model versions, context configuration, and prompt set.
- The experiment does not establish a universal ranking between the models.
- More tasks are required before drawing broader conclusions.

## Status

**Completed — baseline experiment.**

Next evaluation work should expand the benchmark only when it answers a specific engineering question, rather than adding tests for the sake of increasing the benchmark size.
