# 🧠 Gensyn RL-Swarm Node Setup on AWS Ubuntu 24.04

This guide will help you set up and run the [Gensyn RL-Swarm](https://github.com/gensyn-ai/rl-swarm) node on an AWS Ubuntu 24.04 VPS (`t3.xlarge` or higher recommended).

---

## ✅ Requirements
- AWS EC2 instance running Ubuntu 24.04
- Swarm PEM key (for node authentication)

---

## 🔹 1. Connect to Your VPS

```bash
ssh ubuntu@your-ip-address
```

---

## 🔹 2. System Preparation

Update system & install required tools:

```bash
sudo apt update && sudo apt install -y python3 python3-venv python3-pip curl wget screen git lsof
```

---

## 🔹 3. Install Node.js, npm & Yarn

```bash
# Node.js (v20)
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt update && sudo apt install -y nodejs

# Yarn
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list > /dev/null
sudo apt update && sudo apt install -y yarn
```

### ✅ Check Versions
```bash
node -v
npm -v
yarn -v
python3 --version
```

---

## 🔹 4. Clone RL-Swarm Repository

```bash
git clone https://github.com/gensyn-ai/rl-swarm.git
cd rl-swarm
```

---

## 🔹 5. Start a Screen Session

```bash
screen -S gensyn
```

> Detach: `Ctrl + A`, then `D`

---

## 🔹 6. Setup Python Virtual Environment

```bash
python3 -m venv .venv
source .venv/bin/activate
```

---

## 🔹 7. Install Frontend Dependencies

```bash
cd modal-login
yarn install
yarn upgrade && yarn add next@latest && yarn add viem@latest
cd ..
```

---

## 🔹 8. Run the RL-Swarm Node

```bash
./run_rl_swarm.sh
```

> When prompted:  
**Would you like to connect to the Testnet? [Y/n]** → Type `Y`

---

## 🔹 9. Handle Web Login (Using `cloudflared`)

### 📶 Enable Port 3000 Access
```bash
sudo apt install ufw -y
sudo ufw allow 3000/tcp
sudo ufw enable
```

### 🌐 Install & Run Cloudflared
```bash
wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared-linux-amd64.deb

cloudflared tunnel --url http://localhost:3000
```

> Open the generated URL in your **local browser**  
> Login → Email → OTP → Wait for "connected"

---

## 🔹 10. Complete Terminal Prompts

- You'll get your `ORG_ID` → **Save this value**
- Prompt to push models → Enter `N`

---

## 🔹 11. Detach & Monitor

### Detach from screen:
```bash
Ctrl + A, then D
```

### Reattach to screen:
```bash
screen -r gensyn
```

---

## ✅ Your Gensyn RL-Swarm Node is Now Running! 🚀

Happy Swarming 💥

