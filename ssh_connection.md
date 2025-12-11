# Setting SSH KEY to connect Linux Server
## Windows
- Step 1. 在 Windows（PowerShell）產生 SSH Key
  - 在powershell輸入 `ssh-keygen -t ed25519 -C "username"`
  - 它會問你 `Enter file in which to save the key (/c/Users/你的帳號/.ssh/id_ed25519):`
  - 直接按 Enter（使用預設路徑）
  - passphrase 想留空就直接 Enter（之後登入不需要密碼）
  - 產生完後會看到：
    - id_ed25519  (私鑰)  ← 絕對不能給別人
    - id_ed25519.pub  (公鑰) ← 要放到 Linux
- Step 2. 把公鑰丟到 Linux server
  - 顯示你的公鑰 `cat $env:USERPROFILE\.ssh\id_ed25519.pub`
  - 複製整串公鑰`ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI... username`
## Linux
- Step 1. SSH進入Linux
  - `ssh username@192.168.xx.xx`
- Step 2. 建立key目錄
  - `mkdir -p ~/.ssh`
  - `chmod 700 ~/.ssh`
- Step 3. 編輯authorized_keys
  - `nano ~/.ssh/authorized_keys`
  - 把剛剛複製的公鑰貼上去 → Ctrl+O 儲存 → 按Enter → Ctrl+X 離開
- Step 4. 設權限
  - `chmod 600 ~/.ssh/authorized_keys`
