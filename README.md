# AI Coding Agent – Calculator & File Operations

This project is a learning-purpose AI agent that can:

- List files in a directory  
- Read the contents of files  
- Write new file contents  
- Execute Python files  
- Run a calculator application  

The agent uses Google’s `gemini-2.0-flash-001` model with function calling enabled to execute safe, pre-declared Python functions inside a controlled working directory (`./calculator`).

> ⚠️ **Security Note:** This project is **not** production-ready. It can execute Python files within the working directory and is meant for local experiments only.

---

## Features

### 1) List Files
Use the `get_files_info` function to list files and directories (with sizes):

```python
get_files_info({'directory': '.'})
```

### 2) Read File Contents
Use the `read_file` function to return the text content of a file:

```python
read_file({'file_path': 'example.txt'})
```

### 3) Write File Contents
Use the `write_file` function to create or update a file:

```python
write_file({'file_path': 'newfile.txt', 'contents': 'Hello World!'})
```

> For safety, avoid overwriting important files.

### 4) Execute Python Files
Use the `run_python_file` function to run Python scripts within the working directory:

```python
run_python_file({'file_path': 'tests.py'})
```

### 5) Calculator Application
The calculator (`main.py` in the `calculator` directory) can parse and execute arithmetic operations:

```bash
uv run calculator/main.py "3 + 5"
```

---

## How It Works

1. Functions are declared with `types.FunctionDeclaration` and grouped with `types.Tool`.  
2. These tools are passed to `gemini-2.0-flash-001` in the `generate_content` call.  
3. The model returns a `function_call` with:
   - `name`: the function name as a string  
   - `args`: a dictionary of arguments  
4. The call is routed through `call_function()` which:
   - Injects the working directory (`./calculator`)  
   - Executes the mapped Python function (`get_files_info`, `read_file`, `write_file`, `run_python_file`)  
   - Returns a structured `types.Content` via `Part.from_function_response(...)`

---

## Example CLI Usage

List files in the root directory:
```bash
uv run main.py "what files are in the root?"
```

List files in `pkg`:
```bash
uv run main.py "what files are in the pkg directory?"
```

Read the contents of `example.txt`:
```bash
uv run main.py "read lorem.txt"
```

Write a new file:
```bash
uv run main.py "write hello.txt with the text 'Hello AI Agent!'"
```

Run calculator tests:
```bash
uv run main.py "run the calculator tests"
```

Verbose mode (prints function calls and results):
```bash
uv run main.py "list the directory contents" --verbose 
```

---

## Requirements

- Python 3.10+  
- `uv` (for running commands)  
- Google GenAI Python SDK (`google-genai`)  

---

## Security Considerations

- All file operations and Python execution are restricted to the `./calculator` directory.  
- Execution timeout is 30 seconds to prevent infinite loops.  
- Do **not** share this agent with untrusted users.

---

## License

This project is a Boot.dev project for **educational purposes only**.
