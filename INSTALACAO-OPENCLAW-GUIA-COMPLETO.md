# 🦞 INSTALAÇÃO OPENCLAW - GUIA COMPLETO

## 📋 INFORMAÇÕES DO SERVIDOR
```
IP: SEU_IP_VPS
Usuário: root
Senha: SUA_SENHA_AQUI
Porta SSH: 22
```

---

## 🎯 RESUMO EXECUTIVO - O QUE VAMOS FAZER

1. ✅ Atualizar sistema e instalar dependências
2. ✅ Instalar Node.js 22.x (requisito obrigatório)
3. ✅ Instalar pnpm (gerenciador de pacotes)
4. ✅ Instalar OpenClaw via NPM
5. ✅ Configurar API Keys (Anthropic)
6. ✅ Executar wizard de onboarding
7. ✅ Configurar como serviço systemd
8. ✅ Configurar firewall/portas
9. ✅ Testar instalação
10. ✅ Configurar canais (WhatsApp/Telegram)

**Tempo estimado:** 20-30 minutos

---

## 📝 SCRIPT COMPLETO DE INSTALAÇÃO

Copie este script completo e execute no servidor:

```bash
#!/bin/bash
# ========================================
# OPENCLAW - INSTALAÇÃO AUTOMATIZADA
# Servidor: SEU_IP_VPS
# Data: $(date)
# ========================================

set -e  # Parar em caso de erro

echo "🦞 Iniciando instalação do OpenClaw..."

# ========================================
# 1. ATUALIZAR SISTEMA
# ========================================
echo "📦 Atualizando sistema..."
apt update -y
apt upgrade -y

# ========================================
# 2. INSTALAR DEPENDÊNCIAS BÁSICAS
# ========================================
echo "🔧 Instalando dependências..."
apt install -y \
    curl \
    wget \
    git \
    build-essential \
    ca-certificates \
    gnupg \
    lsb-release \
    software-properties-common \
    chromium-browser \
    libgbm1 \
    libnss3 \
    libatk1.0-0 \
    libatk-bridge2.0-0 \
    libcups2 \
    libdrm2 \
    libxkbcommon0 \
    libxcomposite1 \
    libxdamage1 \
    libxfixes3 \
    libxrandr2 \
    libgbm1 \
    libasound2

# ========================================
# 3. INSTALAR NODE.JS 22.x
# ========================================
echo "📦 Instalando Node.js 22.x..."

# Remover versões antigas se existirem
apt remove -y nodejs npm 2>/dev/null || true

# Adicionar repositório oficial do Node.js
curl -fsSL https://deb.nodesource.com/setup_22.x | bash -

# Instalar Node.js
apt install -y nodejs

# Verificar instalação
node --version
npm --version

# ========================================
# 4. INSTALAR PNPM
# ========================================
echo "📦 Instalando pnpm..."
npm install -g pnpm

# Verificar
pnpm --version

# ========================================
# 5. INSTALAR OPENCLAW
# ========================================
echo "🦞 Instalando OpenClaw globalmente..."
npm install -g openclaw@latest

# Verificar instalação
openclaw --version

# ========================================
# 6. CRIAR DIRETÓRIOS DE TRABALHO
# ========================================
echo "📁 Criando estrutura de diretórios..."
mkdir -p ~/.openclaw
mkdir -p ~/.openclaw/workspace
mkdir -p ~/.openclaw/logs
mkdir -p ~/.openclaw/credentials

# ========================================
# 7. CONFIGURAR VARIÁVEIS DE AMBIENTE
# ========================================
echo "🔧 Configurando variáveis de ambiente..."

cat > ~/.openclaw/env.sh <<'EOF'
# OpenClaw Environment Variables
export OPENCLAW_HOME="$HOME/.openclaw"
export OPENCLAW_WORKSPACE="$HOME/.openclaw/workspace"
export OPENCLAW_PORT=18789
export OPENCLAW_HOST="0.0.0.0"
export NODE_ENV="production"
EOF

# Adicionar ao bashrc se não existir
if ! grep -q "openclaw/env.sh" ~/.bashrc; then
    echo "source ~/.openclaw/env.sh" >> ~/.bashrc
fi

source ~/.openclaw/env.sh

# ========================================
# 8. FIREWALL - ABRIR PORTAS
# ========================================
echo "🔥 Configurando firewall..."

# Verificar se ufw está instalado
if command -v ufw &> /dev/null; then
    ufw allow 18789/tcp comment "OpenClaw Gateway"
    ufw allow 22/tcp comment "SSH"
    ufw --force enable
    ufw status
else
    echo "⚠️  UFW não instalado - pulando configuração de firewall"
fi

echo ""
echo "✅ Instalação base concluída!"
echo ""
echo "📊 VERSÕES INSTALADAS:"
echo "   Node.js: $(node --version)"
echo "   NPM: $(npm --version)"
echo "   pnpm: $(pnpm --version)"
echo "   OpenClaw: $(openclaw --version)"
echo ""
echo "🎯 PRÓXIMO PASSO: Execute o wizard de configuração"
echo "   Comando: openclaw onboard --install-daemon"
echo ""
```

---

## 🎯 PASSO A PASSO MANUAL (Se preferir fazer manual)

### **PASSO 1: Conectar ao Servidor**
```bash
ssh root@SEU_IP_VPS
# Senha: SUA_SENHA_AQUI
```

### **PASSO 2: Atualizar Sistema**
```bash
apt update -y && apt upgrade -y
```

### **PASSO 3: Instalar Dependências**
```bash
apt install -y curl wget git build-essential ca-certificates gnupg \
    chromium-browser libgbm1 libnss3 libatk1.0-0 libatk-bridge2.0-0 \
    libcups2 libdrm2 libxkbcommon0 libxcomposite1 libxdamage1 \
    libxfixes3 libxrandr2 libasound2
```

### **PASSO 4: Instalar Node.js 22**
```bash
# Adicionar repositório
curl -fsSL https://deb.nodesource.com/setup_22.x | bash -

# Instalar
apt install -y nodejs

# Verificar
node --version  # Deve mostrar v22.x.x
npm --version
```

### **PASSO 5: Instalar pnpm**
```bash
npm install -g pnpm
pnpm --version
```

### **PASSO 6: Instalar OpenClaw**
```bash
npm install -g openclaw@latest
openclaw --version
```

### **PASSO 7: Configurar Firewall**
```bash
# Abrir porta do Gateway
ufw allow 18789/tcp comment "OpenClaw Gateway"
ufw allow 22/tcp comment "SSH"
ufw --force enable
ufw status
```

---

## ⚙️ CONFIGURAÇÃO INICIAL - WIZARD

### **EXECUTAR WIZARD INTERATIVO**
```bash
openclaw onboard --install-daemon
```

### **O QUE O WIZARD VAI PERGUNTAR:**

#### **1️⃣ Gateway Type**
```
Escolha: Local
```

#### **2️⃣ Model Provider**
```
Escolha: Anthropic
API Key: [Você precisa ter uma Anthropic API Key]
```

💡 **Se você ainda não tem Anthropic API Key:**
- Acesse: https://console.anthropic.com/
- Crie uma conta
- Gere uma API Key
- **GUARDE A KEY** - você só vê ela uma vez

#### **3️⃣ Brave Search (Opcional)**
```
Recomendado: SIM
API Key: [Brave Search API Key]
```

💡 **Obter Brave Search Key (Grátis):**
- Acesse: https://brave.com/search/api/
- Crie conta e gere key

#### **4️⃣ Channels - WhatsApp**
```
Escolha: SIM (se quiser usar WhatsApp)
Método: QR Code
```

**O wizard vai:**
- Gerar um QR Code no terminal
- Você escaneia com WhatsApp → Configurações → Aparelhos conectados
- **IMPORTANTE:** Use número secundário/comercial

#### **5️⃣ Channels - Telegram**
```
Escolha: SIM (se quiser usar Telegram)
Bot Token: [Cole o token do @BotFather]
```

💡 **Criar Bot Telegram:**
```
1. No Telegram, fale com @BotFather
2. Envie: /newbot
3. Escolha nome e username
4. Copie o token fornecido
5. Cole no wizard
```

#### **6️⃣ Channels - Discord**
```
Escolha: SIM (se quiser Discord)
Bot Token: [Token do Discord Developer Portal]
```

#### **7️⃣ Install as Daemon**
```
Escolha: YES
Runtime: Node (OBRIGATÓRIO para WhatsApp/Telegram)
Service Manager: systemd
```

---

## 🔐 CONFIGURAÇÃO MANUAL DE API KEYS (Alternativa ao Wizard)

Se preferir configurar manualmente depois:

```bash
# Configurar Anthropic API Key
openclaw configure --section auth

# Configurar Brave Search
openclaw configure --section web

# Ver configuração atual
cat ~/.openclaw/config.json
```

---

## 📝 ARQUIVO DE CONFIGURAÇÃO EXEMPLO

Arquivo: `~/.openclaw/config.json`

```json
{
  "gateway": {
    "host": "0.0.0.0",
    "port": 18789,
    "auth": {
      "token": "SEU_TOKEN_AQUI"
    }
  },
  "agents": {
    "defaults": {
      "model": "claude-sonnet-4-5-20250929",
      "workspace": "~/.openclaw/workspace",
      "sandbox": {
        "mode": "non-main"
      }
    }
  },
  "tools": {
    "web": {
      "search": {
        "apiKey": "SUA_BRAVE_API_KEY"
      }
    }
  },
  "channels": {
    "whatsapp": {
      "enabled": true
    },
    "telegram": {
      "enabled": true,
      "token": "SEU_TELEGRAM_BOT_TOKEN"
    }
  }
}
```

---

## 🚀 INICIAR SERVIÇO

### **Se instalou como daemon:**
```bash
# Status
systemctl status openclaw

# Iniciar
systemctl start openclaw

# Parar
systemctl stop openclaw

# Reiniciar
systemctl restart openclaw

# Logs em tempo real
journalctl -u openclaw -f
```

### **Iniciar manualmente (teste):**
```bash
openclaw gateway --port 18789 --verbose
```

---

## ✅ VERIFICAÇÃO PÓS-INSTALAÇÃO

### **1. Verificar Status**
```bash
openclaw status
openclaw health
openclaw gateway status
```

### **2. Acessar Dashboard Web**
```bash
# Localmente no servidor
curl http://localhost:18789

# Via browser externo
http://SEU_IP_VPS:18789
```

💡 **Se você configurou token de autenticação:**
- Primeiro acesso vai pedir o token
- Token está em: `~/.openclaw/config.json` → `gateway.auth.token`

### **3. Testar Channels**
```bash
# Listar canais configurados
openclaw channels list

# Status WhatsApp
openclaw channels status whatsapp

# Status Telegram
openclaw channels status telegram
```

### **4. Enviar Mensagem de Teste**
```bash
openclaw message send --target +5511999999999 --message "Teste OpenClaw"
```

### **5. Ver Logs**
```bash
# Logs do gateway
openclaw logs gateway

# Logs gerais
openclaw logs

# Logs com filtro
openclaw logs --level error
```

---

## 🔒 SEGURANÇA - CONFIGURAÇÕES IMPORTANTES

### **1. Firewall Básico**
```bash
# Permitir apenas portas necessárias
ufw default deny incoming
ufw default allow outgoing
ufw allow 22/tcp    # SSH
ufw allow 18789/tcp # OpenClaw
ufw enable
```

### **2. Mudar Porta SSH (Recomendado)**
```bash
# Editar configuração SSH
nano /etc/ssh/sshd_config

# Alterar linha:
Port 2222  # ou outra porta de sua escolha

# Reiniciar SSH
systemctl restart sshd

# Atualizar firewall
ufw allow 2222/tcp
ufw delete allow 22/tcp
```

### **3. Pairing de DMs (Segurança)**
```bash
# Listar pairings pendentes
openclaw pairing list

# Aprovar pairing
openclaw pairing approve whatsapp <código>

# Rejeitar pairing
openclaw pairing reject whatsapp <código>
```

### **4. Auditoria de Segurança**
```bash
openclaw security audit --deep
```

---

## 🔧 COMANDOS ÚTEIS DO DIA A DIA

### **Gestão do Serviço**
```bash
# Status completo
openclaw status --all

# Reiniciar gateway
openclaw gateway restart

# Ver configuração
openclaw configure --view

# Editar configuração
nano ~/.openclaw/config.json
```

### **Logs e Debug**
```bash
# Logs em tempo real
openclaw logs -f

# Logs com timestamp
openclaw logs --timestamps

# Logs de erro apenas
openclaw logs --level error
```

### **Channels**
```bash
# Login WhatsApp (gerar novo QR)
openclaw channels login

# Logout WhatsApp
openclaw channels logout whatsapp

# Status de todos canais
openclaw channels list
```

### **Skills/Plugins**
```bash
# Listar skills disponíveis
openclaw skills list

# Instalar skill
openclaw skills install <nome>

# Ver skills instaladas
openclaw skills --installed
```

---

## 📊 MONITORAMENTO

### **Health Check Endpoint**
```bash
curl http://localhost:18789/health
```

### **Dashboard Web**
```
http://SEU_IP_VPS:18789/dashboard
```

### **Logs do Sistema**
```bash
# Ver logs do systemd
journalctl -u openclaw -n 100

# Logs em tempo real
journalctl -u openclaw -f

# Logs com filtro de erro
journalctl -u openclaw -p err
```

---

## 🐛 TROUBLESHOOTING COMUM

### **Gateway não inicia**
```bash
# Verificar se porta está em uso
lsof -i :18789

# Matar processo na porta
kill -9 $(lsof -t -i:18789)

# Ver erro detalhado
openclaw gateway --verbose
```

### **WhatsApp não conecta**
```bash
# Re-logar
openclaw channels logout whatsapp
openclaw channels login

# Limpar sessão
rm -rf ~/.openclaw/.wwebjs_*
openclaw channels login
```

### **Bot não responde**
```bash
# Verificar auth
openclaw health

# Ver status de pairing
openclaw pairing list

# Ver logs
openclaw logs -f
```

### **Erro de permissão**
```bash
# Corrigir permissões
chown -R root:root ~/.openclaw
chmod -R 755 ~/.openclaw
```

---

## 📦 BACKUP E RESTORE

### **Fazer Backup**
```bash
# Backup completo
tar -czf openclaw-backup-$(date +%Y%m%d).tar.gz ~/.openclaw

# Backup apenas config
cp ~/.openclaw/config.json ~/openclaw-config-backup.json
```

### **Restaurar Backup**
```bash
# Restaurar completo
tar -xzf openclaw-backup-YYYYMMDD.tar.gz -C ~/

# Restaurar config
cp ~/openclaw-config-backup.json ~/.openclaw/config.json
```

---

## 🔄 ATUALIZAÇÃO

### **Atualizar OpenClaw**
```bash
# Atualizar via npm
npm update -g openclaw

# Verificar nova versão
openclaw --version

# Reiniciar serviço
systemctl restart openclaw
```

---

## 📋 CHECKLIST FINAL

Depois de concluir a instalação, verifique:

- [ ] Node.js 22+ instalado: `node --version`
- [ ] OpenClaw instalado: `openclaw --version`
- [ ] Gateway rodando: `openclaw gateway status`
- [ ] Firewall configurado: `ufw status`
- [ ] API Keys configuradas: `openclaw health`
- [ ] Canais conectados: `openclaw channels list`
- [ ] Dashboard acessível: `http://SEU_IP_VPS:18789`
- [ ] Pairing configurado: `openclaw pairing list`
- [ ] Logs funcionando: `openclaw logs`
- [ ] Backup criado: `ls ~/*.tar.gz`

---

## 🎯 PRÓXIMOS PASSOS

Após instalação básica funcionar:

1. **Configurar Skills Customizadas**
2. **Integrar com n8n** (workflows OmniForge)
3. **Criar Skills para clientes**
4. **Setup de monitoramento** (Prometheus/Grafana)
5. **Configurar HTTPS** (Nginx/Certbot)
6. **Deploy multi-instância** (se necessário)

---

## 📞 INFORMAÇÕES ÚTEIS

**Documentação Oficial:** https://docs.openclaw.ai/
**GitHub:** https://github.com/openclaw/openclaw
**Discord Community:** https://discord.gg/openclaw
**Skills Hub:** https://clawhub.com

**Portas Usadas:**
- `18789` - Gateway principal
- `22` - SSH (ou porta customizada)

**Arquivos Importantes:**
- `~/.openclaw/config.json` - Configuração principal
- `~/.openclaw/credentials/` - Credenciais OAuth
- `~/.openclaw/workspace/` - Workspace do agente
- `~/.openclaw/logs/` - Logs da aplicação

---

> **Nota:** Este guia foi criado para instalação do OpenClaw no servidor SEU_IP_VPS
