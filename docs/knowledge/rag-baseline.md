\# Personal AI Infrastructure — RAG Baseline



\## Project Identity



The Personal AI Infrastructure project is a self-hosted AI infrastructure platform for local multimodal inference, knowledge retrieval, tool integration, MCP-based workflows, and automation without mandatory commercial API dependencies.



\## Hardware Baseline



The primary development machine uses an Intel Core i7-1185G7 processor, Intel Iris Xe graphics, and approximately 15.4 GB of system memory available to Windows.



\## Local Model Baseline



The primary multimodal model for the initial project baseline is Qwen3-VL 2B configured with a project-specific 4096-token context limit.



\## Runtime



Ollama provides the local model runtime and exposes its local API on port 11434.



Open WebUI provides the local browser-based AI workspace and is running on port 8080.



\## Engineering Principle



Every major component in the project should have a documented purpose, an identified alternative, a reproducible configuration, a measurable test, known limitations, and documented failure cases.

