# 🚀 Fullstack Dev Environment Setup (Arch Linux)

Este projeto fornece um script completo para configurar um ambiente de desenvolvimento fullstack moderno no Arch Linux.

---

## 📦 O que será instalado

### 🔧 Base

* base-devel
* git, curl, wget
* python + pip
* docker

### 🟢 Node.js

* NVM (Node Version Manager)
* Node.js LTS
* npm, yarn, pnpm

### ⚛️ Frontend

* Vite
* React (template pronto)
* TypeScript
* ESLint + Prettier

### 🔙 Backend

* NestJS
* Prisma ORM
* PM2

### 🗄️ Banco de dados

* PostgreSQL
* Redis

---

## ▶️ Instalação

```bash
chmod +x setup-fullstack-complete.sh
./setup-fullstack-complete.sh
```

---

## 📁 Script

```bash
#!/bin/bash

set -e

echo "==> Atualizando sistema..."
sudo pacman -Syu --noconfirm

echo "==> Instalando dependências base..."
sudo pacman -S --needed --noconfirm \
  base-devel git curl wget unzip zip \
  python python-pip docker \
  postgresql redis

# Docker
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker $USER

# PostgreSQL
sudo -u postgres initdb -D /var/lib/postgres/data || true
sudo systemctl enable postgresql
sudo systemctl start postgresql

# Redis
sudo systemctl enable redis
sudo systemctl start redis

# NVM
export NVM_DIR="$HOME/.nvm"
if [ ! -d "$NVM_DIR" ]; then
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
fi

source "$NVM_DIR/nvm.sh"

# Node
nvm install --lts
nvm use --lts
nvm alias default 'lts/*'

# Pacotes globais
npm install -g npm yarn pnpm
npm install -g typescript ts-node nodemon eslint prettier vite create-vite
npm install -g pm2 prisma @nestjs/cli

# Criar projeto exemplo
mkdir -p ~/projects && cd ~/projects

# Frontend
npm create vite@latest frontend -- --template react
cd frontend
npm install
cd ..

# Backend
nest new backend --package-manager npm --skip-git


echo "==> Setup completo finalizado 🚀"
echo "Reinicie a sessão para aplicar docker e nvm corretamente"
```

---

## 🧪 Testando

```bash
node -v
npm -v
docker -v
psql --version
redis-server --version
```

---

## 📌 Observações

* Faça logout/login após instalação (Docker + NVM)
* PostgreSQL pode precisar de configuração adicional
* Projetos são criados em ~/projects

---

## ⭐ Contribuição

Pull requests são bem-vindos!
