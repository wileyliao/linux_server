# Docker 安裝
- Step 1. 更新 apt 與基本套件
```
sudo apt update
sudo apt install -y ca-certificates curl gnupg
```
- Step 2. 加入 Docker 官方 GPG key
```
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
- Step 3. 加入 Docker 官方套件來源
```
echo \
  "deb [arch=$(dpkg --print-architecture) \
  signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
- Step 4. 加入 Docker 官方套件來源
```
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
- Step 5. 驗證 Docker 是否正常
```
docker --version
docker compose version
```
- Step 6. 不用 sudo 就能跑 docker
`sudo usermod -aG docker $USER`

- Step 7. 測試
`docker run hello-world`
