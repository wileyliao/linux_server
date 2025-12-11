# linux_server
Implementation Notes

## Mind map
```
╔══════════════════╦══════════════════════════╗
║ Windows (Client) ║   Linux Server (Engine)  ║
╠══════════════════╬══════════════════════════╣
║ PyCharm UI       ║  SSH Server              ║
║ ─ Code Editor    ║  ─ Executes commands     ║
║ ─ Project files  ║  ─ Hosts your projects   ║
║ ─ Shows logs     ║                          ║
╠══════════════════╬══════════════════════════╣
║ No Python runtime║ Python Runtime(s)        ║
║ No CUDA          ║ ─ venv1 / venv2          ║
║ No FFmpeg        ║ ─ Docker containers      ║
║ No Torch         ║ ─ GPU (CUDA)             ║
╠══════════════════╬══════════════════════════╣
║ SSH to server →  ║ PyCharm connects via SSH ║
║ Runs code THERE  ║ Runs Python THERE        ║
╚══════════════════╩══════════════════════════╝
```

## Dev UI on Windows but Running on linux
### Where is my code?
- Mode A: IDE 自動同步
  - Windows → SFTP → Linux ~/project
- Mode B: Docker volume
  - Linux ~/project → volume → container:/app
### Where is my Python Interpreter?
- Mode A: Linux venv --> `Linux ~/projectA/.venv/bin/python`
  - IDE 用SSH進去跑
- Mode B: Python in Docker container --> `docker exec … /usr/bin/python`
  - IDE 透過 Docker Engine 控制它
- Mode C(advance): Docker compose 裡某個 service --> `docker compose exec ai_api python`
  - IDE 也能當interpreter
### Where is CUDA/GPU/FFMPEG....
- 全部都在 Linux，不在 Windows --> `Linux GPU → Docker → Python → Model → FFmpeg`
- Windows 只是顯示UI & Debug面板

## What does windowsOS do?
- Code writting(IDE)
- Command sending(SSH)
