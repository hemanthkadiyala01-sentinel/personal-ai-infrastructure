\# Experiment 001 — Qwen3-VL 2B Multimodal Baseline



\## Objective



Validate local multimodal inference using Qwen3-VL 2B under the project's CPU-only hardware constraints.



\## Environment



\- Host: Windows

\- CPU: Intel Core i7-1185G7

\- Cores/Threads: 4 cores / 8 logical processors

\- RAM: approximately 15.4 GB visible to Windows

\- GPU: Intel Iris Xe

\- Dedicated VRAM: None

\- Runtime: Ollama 0.34.4

\- Model: qwen3-vl-2b-4k

\- Model architecture: Qwen3-VL

\- Quantization: Q4\_K\_M

\- Context: 4096 tokens

\- Processor: 100% CPU



\## Configuration



The original qwen3-vl:2b model was preserved.



A project-specific Ollama model was created using:



FROM qwen3-vl:2b

PARAMETER num\_ctx 4096



Model:



qwen3-vl-2b-4k



\## Test



An image located at:



C:\\Users\\dell\\Downloads\\1.jpeg



was supplied directly to the Qwen3-VL model through the Ollama interactive interface.



The model reported:



Added image 'C:\\Users\\dell\\Downloads\\1.jpeg'



This confirms that the image was accepted as multimodal input.



\## Observations



The model successfully:



\- accepted the image

\- recognized a hand

\- recognized a transparent card

\- attempted to extract visible text

\- interpreted the general context of the image



The response also demonstrated uncertainty during text interpretation.



The model repeatedly proposed alternative interpretations of unclear text instead of consistently restricting itself to confidently visible information.



\## Result



PASS — multimodal image processing was successfully demonstrated.



LIMITATION — OCR/text interpretation requires further evaluation because uncertain visual text can lead to speculative output.



\## Runtime Verification



While loaded:



qwen3-vl-2b-4k:latest

2.0 GB

100% CPU

4096 context



After the automatic unload period:



No model was shown by `ollama ps`.



This confirms that the model unloaded normally from runtime memory.



\## Engineering Conclusion



Qwen3-VL 2B is operational on the project's CPU-only development machine when constrained to a 4096-token context.



The experiment demonstrates that model download size alone is not sufficient to evaluate local AI feasibility. Runtime memory behavior, context length, processor utilization, latency, and output reliability must also be measured.



Further experiments should benchmark:



\- text generation latency

\- instruction following

\- vision accuracy

\- OCR reliability

\- memory/runtime behavior

\- different context lengths

\- comparison with other local models



\## Status



Experiment completed.

