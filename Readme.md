# 🚀 Llama Server Setup Project

This project provides instructions to set up and launch the Llama Server using Docker.

## ⚙️ Setup Guide

Follow these steps sequentially to get the server running:

### 1. Clone Source Code
Before starting, clone the main repository as it contains necessary packages for the Dockerfile:
```bash
git clone https://github.com/ggml-org/llama.cpp.git
```

### 2. Build Docker Image
Build the required Docker image:
```bash
docker build -t local/llama.cpp:server-cuda --target server -f Dockerfile.llama .
```

### 2.5. Configure Docker (Prerequisite)
**MANDATORY:** Before running the container with GPU support (`--gpus all`), you must configure Docker to recognize your NVIDIA hardware.
Please follow the official installation guide: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html
**Crucial:** Ensure you complete the **Configuring Docker** section of this guide.

### 3. Launch the Llama Server
The following command is used to start the Llama server:
```bash
docker run --network mcp-net \
  -p 8080:8080 --gpus all \
  -v $(pwd):/models \
  local/llama.cpp:server-cuda \
  -m /models/gemma4/gemma-4-E4B-it-qat-UD-Q4_K_XL.gguf \
  --mmproj /models/gemma4/mmproj/mmproj-F16.gguf \
  -c 120000 -n 1024 --top-k 20 --temperature 0.7 -ngl 999 \
  --host 0.0.0.0 --port 8080 --webui-mcp-proxy
```
**Note on Performance:**
The `-c` parameter controls the context size and the amount of VRAM utilized on the GPU. This value can be adjusted as needed. 
On the current build, the performance is quite fast, achieving approximately: `tg = 54.25 t/s, tg_3s = 55.29 t/s`.

## 🖥️ Hardware Used

This configuration was tested on the following hardware:
- **GPU**: 4060ti 8gb laptop
- **CPU**: i7-13700HX × 24
- **RAM**: 16gb

## 📚 Resources

### Docker Documentation
For detailed Docker setup, refer to: https://github.com/ggml-org/llama.cpp/blob/master/docs/docker.md

### Host Configuration
For host setup (e.g., NVIDIA Container Toolkit), refer to: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html