# 🦞 OpenClaw Prime - Bot IA com Telegram

Bot de IA configurado no OpenClaw com integração Telegram, múltiplos modelos e skill de segurança Prime.

## 📋 Configuração Atual

### VPS
- **IP:** SEU_IP_VPS
- **Porta Gateway:** 18789
- **Telegram Bot:** @omnieprimebot

### Modelos Configurados
- **Principal:** `openai/gpt-4.1-mini`
- **Fallback:** `openai/gpt-4o-mini`
- **Fallback 2:** `openrouter/google/gemini-3-flash-preview`

### APIs Configuradas
- ✅ OpenAI
- ✅ OpenRouter
- ✅ ElevenLabs (TTS)

## 🛡️ Skill Prime

Skill de segurança inteligente que:
- Detecta credenciais expostas
- Auto-corrige código inseguro
- Aplica RLS automaticamente
- Referências LGPD, ISO 27001, OWASP

Ver: [prime/SKILL.md](prime/SKILL.md)

## 📁 Estrutura

```
├── prime/                    # Skill de segurança
│   ├── SKILL.md             # Documentação completa
│   ├── patterns.yaml        # Padrões de detecção
│   └── README.md
├── INSTALACAO-OPENCLAW-GUIA-COMPLETO.md  # Guia de instalação
├── openclaw_tunnel_service.py            # SSH tunnel para PC
└── scripts diversos de setup
```

## 🚀 Comandos Úteis

### Verificar Status
```bash
ssh root@SEU_IP_VPS
export XDG_RUNTIME_DIR=/run/user/0
openclaw gateway health
```

### Reiniciar Gateway
```bash
systemctl --user restart openclaw-gateway
```

### Ver Logs
```bash
openclaw logs --follow
```

## 🔧 Scripts de Manutenção

| Script | Função |
|--------|--------|
| `check_status.py` | Verifica status completo |
| `setup_openai.py` | Configura API OpenAI |
| `fix_models.py` | Atualiza modelos |
| `install_prime.py` | Instala skill Prime |

---

**OmniForge Soluções em IA** | Janeiro 2025
