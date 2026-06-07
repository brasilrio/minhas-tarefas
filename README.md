# ✅ Minhas Tarefas

PWA (Progressive Web App) de lista de tarefas com integração ao Google Calendar, prioridades coloridas e reordenação por arrastar.

**🌐 App ao vivo:** https://brasilrio.github.io/minhas-tarefas/

## Funcionalidades

- 🎨 **Prioridades coloridas** — 🔴 Urgente · 🟡 Importante · 🟢 Tranquilo
- ✋ **Arrastar para reordenar** — segura o ícone ≡ e arrasta
- 🗓️ **Google Calendar** — tarefas com data/hora viram eventos no seu calendário
- ⏰ **Alarmes** — disparados nativamente pelo app Google Calendar no Android
- 💾 **Offline** — dados salvos no aparelho via `localStorage` + Service Worker

## Como instalar no Android

1. Abra https://brasilrio.github.io/minhas-tarefas/ no **Chrome**
2. Configure o Client ID do Google em ⚙️ Configuração
3. Menu **⋮ → "Instalar app"**

## Setup do Google Calendar (OAuth)

Veja o passo a passo completo em [DECISAO_E_PLANO.md](DECISAO_E_PLANO.md).

Em resumo:
1. Crie um projeto no [Google Cloud Console](https://console.cloud.google.com)
2. Ative a **Google Calendar API**
3. Crie um **OAuth Client ID** (tipo: Aplicativo da Web)
4. Adicione `https://brasilrio.github.io` como origem autorizada
5. Cole o Client ID no app em ⚙️ Configuração

## Arquivos

| Arquivo | Descrição |
|---------|-----------|
| `index.html` | App completo (UI + lógica + Google Calendar API) |
| `manifest.webmanifest` | Configuração PWA para instalação no Android |
| `sw.js` | Service Worker para funcionamento offline |
| `icon-192.png` / `icon-512.png` | Ícones do app |
| `DECISAO_E_PLANO.md` | Documentação de decisões e passo a passo completo |

## Tecnologias

- HTML + CSS + JavaScript puro (sem framework, sem build)
- Google Identity Services (OAuth 2.0)
- Google Calendar API v3
- PWA: Web App Manifest + Service Worker
