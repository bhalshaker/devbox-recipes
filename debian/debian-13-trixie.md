# 🐧 Debian 13 "Trixie" Post-Install Guide

Welcome to Debian 13! This guide walks you through setting up a powerful, developer-ready workstation after a fresh install.

---

## 🔄 System Update

Keep your system up to date:

```sh
sudo apt update && sudo apt upgrade -y
```

---

## 🛠️ Essential Developer Packages

Install core tools and fonts:

```sh
sudo apt install git curl wget fastfetch mpv gcc make python3 python3-pip unzip zip cargo p7zip ntfs-3g htop ffmpeg \
fonts-firacode fonts-jetbrains-mono fonts-croscore fonts-crosextra-carlito \
fonts-crosextra-caladea fonts-noto fonts-noto-cjk -y
```

---

## 🌍 Arabic Fonts

Add support for Arabic fonts:

```sh
sudo apt install xfonts-intl-arabic fonts-hosny-amiri -y
```

---

## 🧠 Node.js via NVM

Install Node Version Manager:

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
source ~/.bashrc
nvm --version
```

---

## 🌐 Chromium Browser

Install Chromium:

```sh
sudo apt install chromium -y
```

---

## ☕ Java Tools via SDKMAN

Install SDKMAN for JVM tools:

```sh
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
```

---

## 📦 Flatpak & Flathub

Enable Flatpak and Flathub:

```sh
sudo apt install flatpak -y
flatpak remote-add --if-not-exists --user flathub https://flathub.org/repo/flathub.flatpakrepo
```

---

## 🐳 Podman & Podman Desktop

Install container tools:

```sh
sudo apt install -y podman podman-compose crun
flatpak install --user flathub io.podman_desktop.PodmanDesktop
```

---

## ⚙️ Ansible

Install Ansible:

```sh
sudo apt install ansible -y
```

---

## 🌍 Terraform Setup

Add HashiCorp repo and install Terraform:

```sh
sudo apt install -y gnupg
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null

gpg --no-default-keyring --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg --fingerprint

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list

sudo apt update
sudo apt-get install terraform
```

---

## 🧑‍💻 Visual Studio Code

Install VS Code:

```sh
curl -L -o /tmp/vscode.deb "https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64" && \
sudo dpkg -i /tmp/vscode.deb && rm /tmp/vscode.deb
```

---

## 🖥️ VirtualBox Setup

### Step 1: Backup sources list

```sh
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
```

### Step 2: Add VirtualBox repo

Edit sources list:

```sh
sudo nano /etc/apt/sources.list
```

Add this line:

```plaintext
# VirtualBox
deb [arch=amd64 signed-by=/usr/share/keyrings/oracle-virtualbox-2016.gpg] https://download.virtualbox.org/virtualbox/debian trixie contrib
```

### Step 3: Import GPG key

```sh
wget -O- https://www.virtualbox.org/download/oracle_vbox_2016.asc | \
sudo gpg --yes --output /usr/share/keyrings/oracle-virtualbox-2016.gpg --dearmor
```

### Step 4: Install VirtualBox

```sh
sudo apt update -y
sudo apt install virtualbox-7.2 -y
```

### Step 5: Secure Boot Fix (if needed)

If installation fails due to Secure Boot:

```sh
sudo mkdir -p /var/lib/shim-signed/mok
```

Generate signing key:

```sh
sudo openssl req -nodes -new -x509 -newkey rsa:2048 -outform DER \
-addext "extendedKeyUsage=codeSigning" \
-keyout /var/lib/shim-signed/mok/MOK.priv \
-out /var/lib/shim-signed/mok/MOK.der
```

Import key:

```sh
sudo mokutil --import /var/lib/shim-signed/mok/MOK.der
sudo reboot
```

After reboot, follow BIOS prompts to enroll the key.

Reinstall VirtualBox:

```sh
sudo apt remove virtualbox-7.2 -y
sudo apt install virtualbox-7.2 -y
```

---

## 🧰 Bonus Developer Tools

### 🐍 Python Dev Essentials

```sh
sudo apt install python3-venv python3-dev build-essential -y
```

### 🧪 Postman (API Testing)

```sh
flatpak install flathub com.getpostman.Postman
```

### 🧮 SQLite & GUI

```sh
sudo apt install sqlite3 sqlitebrowser -y
```

### 🧱 Redis CLI

```sh
sudo apt install redis-tools -y
```