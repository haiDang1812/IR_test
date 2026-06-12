# Download git
```
https://github.com/git-for-windows/git/releases/download/v2.54.0.windows.1/Git-2.54.0-64-bit.exe
```

# Setup env
```bash
pip install uv
uv sync
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
.venv/Scripts/activate
```

If the Set-ExecutionPolicy above doesn't work, use this instead
```bash
Set-ExecutionPolicy -Scope Process -ExecutionPolicy ByPass
```

# Download embedding model
### Choose model in download_model.py
```bash
python download_model.py
```

# Server Config

### `Teacher server (LLM url)`
| File | New value |
|------|----------|
| [main.py](main.py#L17) | `given by teacher` |
| [trigger.py](trigger.py#L25) | `given by teacher` |

### `StudentID (for register)`
| File | New value |
|------|----------|
| [main.py](main.py#L19) | `your student id` |
| [trigger.py](trigger.py#L24) | `your student id` |

### `Student server`
#### Run `ipconfig` in CMD, then use the IPv4 address under the Ethernet section as the Student server URL.


# Start server
```
uvicorn main:app --reload --port 5000 --host 0.0.0.0
```

# Trigger module
### Register
```bash
python trigger.py register --host {student_ipv4} --port {student_server_port}
```

> **Note:**
> - `student_ipv4` — IPv4 address from Ethernet section of `ipconfig`
> - `student_server_port` — port defined when run uvicorn command

### Evalue
For the first evaluate time, set the value of 'document_received' [trigger.py](trigger.py#L80) is False

From the second time, set it as True
```bash
python trigger.py evaluate
```

### Reset (reset before re-evaluate)
```bash
python trigger reset
```

# Modify inference config
| Config | File |
|------|----------|
| Embedding model | [main.py](main.py#L13) | 
| Chunking size | [main.py](main.py#L21) |
| Chunking overlap | [main.py](main.py#L22) |
| Top K | [main.py](main.py#L23) |