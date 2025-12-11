# Ubuntu 安裝 NVIDIA Driver + Docker GPU 支援流程
## 安裝 NVIDIA Driver（主機顯示卡驅動）
- Ubuntu 自帶工具抓正確版本，只需要
```
sudo ubuntu-drivers autoinstall
sudo reboot
```
- 重啟後驗證，輸入`nvidia-smi`見到以下圖表即成功
```
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 580.95.05              Driver Version: 580.95.05      CUDA Version: 13.0     |
+-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce RTX 3060        Off |   00000000:01:00.0 Off |                  N/A |
|  0%   43C    P8              9W /  170W |       1MiB /   8192MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI              PID   Type   Process name                        GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|  No running processes found                                                             |
+-----------------------------------------------------------------------------------------+
```
## 安裝 NVIDIA Container Toolkit（讓 Docker 使用 GPU）
- 清除舊的錯誤 repo（預防）
```
sudo rm /etc/apt/sources.list.d/nvidia-container-toolkit.list
sudo rm /etc/apt/sources.list.d/libnvidia-container.list 2>/dev/null
```
- 下載 NVIDIA GPG Key
```
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey \
  | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
```
- 加入 Repository（帶 signed-by，Ubuntu 24.04 必須加這段）
```
curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list \
  | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' \
  | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```
- 更新來源 & 安裝toolkit
```
sudo apt update
sudp apt install -y nvidia-container-toolkit
```
- 啟用 Docker GPU Runtime
```
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```
  - Docker daemon 支援 `--gpus`
  - Container 正確掛入 GPU driver
- 測試 GPU 容器是否正常
```
docker run --rm --gpus all nvidia/cuda:12.2.0-base-ubuntu22.04 nvidia-smi
```
### 看到跟上面 `nvidia-smi`一樣的圖表，就成功了
