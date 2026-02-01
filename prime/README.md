# 🛡️ Prime

**Sistema de Segurança Inteligente para Automações e Agentes IA**

[![OmniForge](https://img.shields.io/badge/OmniForge-Security-blue)](https://omniforge.com.br)
[![Version](https://img.shields.io/badge/version-1.0-green)](./SKILL.md)

---

## ⚡ Quick Start

O Prime é **ativado automaticamente** quando presente no diretório de skills do OpenClaw.

---

## 🎯 O que faz?

| Funcionalidade | Descrição |
|----------------|-----------|
| 🔍 **Detecção Automática** | Escaneia credenciais, vulnerabilidades e configs inseguras |
| 🔧 **Auto-Correção** | Gera código seguro por padrão (RLS, prepared statements, etc.) |
| 📚 **Educação** | Explica riscos com referências (LGPD, ISO 27001, OWASP) |
| ⚖️ **Contextual** | Ajusta severidade (dev vs prod) |

---

## 📁 Estrutura

```
prime/
├── SKILL.md        # Documentação completa e instruções de comportamento
├── patterns.yaml   # Padrões de detecção (regex, severidades)
└── README.md       # Este arquivo
```

---

## 🚨 Níveis de Alerta

| Nível | Quando | Ação |
|-------|--------|------|
| 🔴 **CRÍTICO** | Credencial exposta, RCE | PARE, alerte, não prossiga |
| 🟠 **ALTO** | SQL Injection, XSS, sem RLS | Alerte + corrija |
| 🟡 **MÉDIO** | Logs insuficientes, deps antigas | Informe no final |
| 🟢 **BAIXO** | Hardening extra | Dica se relevante |

---

## 🇧🇷 Compliance

- **LGPD** - Lei Geral de Proteção de Dados
- **ISO 27001:2022** - Segurança da Informação
- **OWASP Top 10** - Vulnerabilidades Web

---

## 🔌 Integrações Suportadas

- Supabase (RLS automático)
- n8n (webhooks seguros)
- Evolution API / WhatsApp
- Chatwoot
- Docker / Portainer
- APIs REST/GraphQL

---

## 📖 Documentação Completa

Consulte [SKILL.md](./SKILL.md) para:

- Regras de execução detalhadas
- Todos os padrões de detecção
- Checklists de hardening
- Exemplos de uso
- Referências normativas completas

---

## 🏢 OmniForge

Desenvolvido por **OmniForge Soluções em IA**

> *"Segurança por design, não por acidente."*

---

**Versão:** 1.0  
**Última atualização:** Janeiro 2025
