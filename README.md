# Secure LLM Agent Design Patterns: A Proof of Concept

## Project Overview

This project serves as a hands-on, simplified demonstration of various security design patterns aimed at protecting Large Language Model (LLM) agents from prompt injection attacks. In an era where LLMs are increasingly integrated into applications with access to powerful tools, understanding and mitigating prompt injection vulnerabilities is crucial. This repository provides practical examples for AI cybersecurity professionals, developers, and students to explore core vulnerabilities and the architectural patterns designed to neutralize them.

**The Core Problem: Prompt Injection**
A prompt injection occurs when an attacker embeds malicious instructions within data that an LLM processes. If the LLM is connected to tools (e.g., file deletion, email sending), it can be tricked into executing harmful actions without explicit user consent. This project illustrates how to build agents resilient to such threats.

## Key Concepts

This project explores several key security patterns for LLM agents:

*   **Prompt Injection:** A type of attack where malicious instructions are inserted into an LLM's input, overriding its original directives.
*   **Dual LLM Pattern:** Separating a "Worker LLM" (handles untrusted data) from a "Manager LLM" (performs risky actions), ensuring the Manager never directly processes untrusted input.
*   **Code-Then-Execute Pattern:** The agent's plan is generated as code *before* exposure to malicious data, and then executed by a secure interpreter.
*   **Action-Selector Pattern:** The agent's capabilities are strictly limited to a predefined, hard-coded list of safe actions, preventing execution of unauthorized operations.
*   **Plan-Then-Execute Pattern:** A Planner LLM creates a step-by-step plan, which is then executed by a separate, non-LLM Executor. The plan is static and created before any interaction with potentially malicious data.
*   **Context-Minimisation Pattern:** A "Filter LLM" extracts only the absolutely necessary information from untrusted data, ensuring the main, privileged LLM never sees malicious instructions.
*   **LLM Map-Reduce Pattern:** Large, untrusted data is processed in isolated chunks by sandboxed "Mapper LLMs," and their sanitized outputs are then combined by a "Reducer LLM," preventing widespread injection.

## Installation Instructions

This project uses `uv` for dependency management.

### Prerequisites

*   Python 3.9+
*   `uv` (install with `pip install uv`)
*   Ollama (for running local LLMs, e.g., Gemma. Download from [ollama.com](https://ollama.com/))

### Step-by-step Setup

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-username/secure_agent_poc.git
    cd secure_agent_poc
    ```

2.  **Create the virtual environment:**
    ```bash
    uv venv
    ```

3.  **Activate the environment:**
    ```bash
    source .venv/bin/activate
    ```
    *On Windows, use:*
    ```bash
    .venv\Scripts\activate
    ```

4.  **Install dependencies:**
    ```bash
    uv pip install -r requirements.txt
    ```

5.  **Download the LLM (Gemma 2B):**
    This project uses `gemma:2b` by default for local LLM interactions via Ollama.
    ```bash
    ollama run gemma:2b
    ```
    *Note: You can change the `MODEL` variable in each `app_levelX.py` file to use a different model if desired (e.g., `gemma:7b`, `llama2`).*

## Usage Guide

Each `app_levelX.py` file demonstrates a specific security pattern.

### Running Demonstrations

To run a specific level's demonstration:

```bash
python app_levelX.py
```

Replace `X` with the desired level number (1-7). For example:

*   **Level 1 (Dual LLM Simulated):**
    ```bash
    python app_level1.py
    ```
*   **Level 2 (Dual LLM Live LLM):**
    ```bash
    python app_level2.py
    ```
*   **Level 3 (Code-Then-Execute):**
    ```bash
    python app_level3.py
    ```
*   **Level 4 (Action-Selector):**
    ```bash
    python app_level4.py
    ```
*   **Level 5 (Plan-Then-Execute):**
    ```bash
    python app_level5.py
    ```
*   **Level 6 (Context-Minimisation):**
    ```bash
    python app_level6.py
    ```
*   **Level 7 (LLM Map-Reduce):**
    ```bash
    python app_level7.py
    ```

Each script will print output to the console, demonstrating the pattern's behavior and how it handles (or mitigates) a simulated prompt injection attempt.

## Configuration and Customization

*   **LLM Model:** The `MODEL` variable at the top of each `app_levelX.py` file specifies the LLM used (e.g., `MODEL = "gemma:2b"`). You can change this to any model available via your Ollama instance.
*   **Ollama Base URL:** The `client = OpenAI(base_url="http://localhost:11434/v1", api_key="ollama")` line configures the connection to your local Ollama server. Adjust `base_url` if your Ollama instance is running on a different address or port.
*   **Simulated Attacks:** Each `app_levelX.py` contains a `malicious_text` variable or similar setup that simulates a prompt injection. You can modify this to experiment with different injection techniques.
*   **Allowed Actions (Levels 4 & 5):** In `app_level4.py` and `app_level5.py`, the `ALLOWED_ACTIONS` dictionary defines the safe functions the agent can call. Adding dangerous functions here would compromise security.

## Examples and Tutorials

Each `app_levelX.py` file serves as a self-contained example demonstrating a specific pattern.

*   **Basic Usage:** Simply run any `app_levelX.py` script as described in the Usage Guide. The console output will guide you through the demonstration.
*   **Understanding the Code:**
    *   Open any `app_levelX.py` file.
    *   Pay attention to the comments explaining the "Configuration," "Pattern Implementation," and "Demonstration" sections.
    *   Observe how the `user_instruction` or `user_query` interacts with the LLM(s) and the defined safe actions.
    *   Crucially, examine the "Final State Check" at the end of each script to see if the malicious command was successfully prevented.

## Troubleshooting

*   **`ollama` command not found:** Ensure Ollama is installed and its executable is in your system's PATH.
*   **`uv` command not found:** Ensure `uv` is installed (`pip install uv`) and in your PATH.
*   **`openai.APIConnectionError`:**
    *   Verify Ollama is running (`ollama list` should show your downloaded models).
    *   Check if the `base_url` in the Python scripts matches your Ollama server's address and port.
    *   Ensure your firewall isn't blocking the connection to `localhost:11434`.
*   **LLM response issues (e.g., not following instructions):**
    *   The `MODEL` specified might not be capable enough for the task. Try a larger model (e.g., `gemma:7b`).
    *   Adjust the `temperature` parameter in the `client.chat.completions.create` calls. Lower values (closer to 0) make the LLM more deterministic and instruction-following.
*   **`FileNotFoundError` for `secret_notes.txt` or other files:** Ensure you are running the scripts from the project's root directory. The scripts create these files dynamically.

## Contribution Guidelines

Contributions are welcome! If you'd like to contribute to this project, please follow these steps:

1.  **Fork the repository.**
2.  **Create a new branch** for your feature or bug fix (`git checkout -b feature/your-feature-name`).
3.  **Make your changes.**
4.  **Write clear, concise commit messages.**
5.  **Test your changes thoroughly.**
6.  **Open a Pull Request** to the `main` branch of this repository, describing your changes and their benefits.

For bug reports or feature requests, please open an issue on the GitHub repository.

## License and Credits

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

**Credits:**

*   Inspired by and based on concepts from various LLM security research and best practices.
*   Uses the `ollama` library for local LLM inference.
*   Uses the `uv` tool for fast dependency management.