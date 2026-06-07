# App de Teste — Decisão de Plataforma e Plano

> Documento de raciocínio e planejamento. Atualizado em 2026-06-07.

## 1. Contexto do usuário

- Tem **iPhone** (uso diário, principal).
- Tem **tablet e celular Android** (uso menos frequente).
- Quer fazer **um app simples, como teste/aprendizado**.
- Restrição importante: **baixo custo de tokens** e **fácil de implementar**.

## 2. Decisão de plataforma

### A pergunta: "é mais fácil instalar apps em Android?" — **Sim, correto.**

| Item | Android | iPhone (iOS) |
|------|---------|--------------|
| Instalar app feito por você | Instala o arquivo `.apk` direto no aparelho ("sideload"), de graça | Precisa de Mac + Xcode + assinatura; conta paga ($99/ano) p/ instalar "de verdade" |
| Custo p/ publicar na loja | Google Play: US$ 25 (uma vez) | Apple: US$ 99 por ano |
| Liberdade para testar | Alta | Restrita |

**Conclusão:** para testar e aprender, **Android ganha** — você consegue rodar o app no seu próprio aparelho sem pagar nada e sem Mac.

### Mas tem um detalhe esperto 👇

A forma **mais barata em tokens e mais simples de programar** é fazer um **PWA (Progressive Web App)**: um app feito em **HTML + CSS + JavaScript em um único arquivo**, que o navegador instala como se fosse um app nativo ("Adicionar à tela inicial").

Vantagens do PWA para o nosso caso:
- **Custo de token mínimo**: é só 1 arquivo, sem build, sem Android Studio, sem Gradle.
- Funciona **no Android** (instala como app de verdade).
- **Bônus**: funciona também **no seu iPhone** (que é o que você usa todo dia) — abre no Safari → Compartilhar → "Adicionar à Tela de Início".
- Zero custo de loja, zero assinatura.

> Se mais para frente você quiser um `.apk` "de verdade" para distribuir, dá para "empacotar" esse mesmo PWA com ferramentas como **PWABuilder** (gera o `.apk` automaticamente) ou um **WebView** simples no Android Studio. Ou seja: começamos leve e só subimos de nível se precisar.

**Plataforma escolhida: Android, via PWA (web app instalável).**

## 3. Três opções de app fáceis de implementar

Todas são de **tela única**, sem servidor/backend, salvando dados no próprio aparelho (`localStorage`). Isso mantém o custo de implementação e de tokens baixíssimo.

### Opção A — Lista de Tarefas (To-Do)
- Adicionar tarefa, marcar como feita, apagar.
- Salva no aparelho (não perde ao fechar).
- **Por que é boa:** o "hello world" dos apps; cobre os fundamentos (input, lista, estado, persistência).
- Dificuldade: ⭐ (mais fácil)

### Opção B — Contador de Hábitos / Água
- Botões "+1" para hábitos do dia (copos de água, exercício, etc.).
- Reseta a cada dia; mostra histórico simples.
- **Por que é boa:** útil no dia a dia, visual bonito, ensina datas e contadores.
- Dificuldade: ⭐⭐

### Opção C — Calculadora de Gorjeta / Dividir a Conta
- Digita valor da conta, % de gorjeta e nº de pessoas → mostra quanto cada um paga.
- **Por que é boa:** lógica de cálculo em tempo real, ótimo para aprender formulários reativos.
- Dificuldade: ⭐ (mais fácil)

## 4. Recomendação

Começar pela **Opção A (Lista de Tarefas)** como PWA:
- É o melhor equilíbrio entre "aprende o essencial" e "termina rápido".
- Um único arquivo `index.html`.
- Testamos primeiro no navegador do PC, depois instalamos no Android (e, se quiser, no iPhone).

## 5. Próximos passos (quando você decidir)

1. Você escolhe a opção (A, B ou C).
2. Eu gero o arquivo `index.html` completo.
3. Você abre no navegador do Android → menu → "Adicionar à tela inicial".
4. Pronto: app instalado e funcionando offline.

## 6. App escolhido: Lista de Tarefas (turbinada)

Decisões do usuário (2026-06-07):
- **Integração Google Calendar:** Completa, via login OAuth (cria/edita/remove eventos automaticamente).
- **Aparelhos:** Foco só no Android.

### Recursos implementados
- ✅ Adicionar / concluir / apagar tarefas (salvas no aparelho, `localStorage`).
- 🎨 **Prioridades coloridas:** 🔴 Urgente · 🟡 Importante · 🟢 Tranquilo. Toca no círculo da tarefa para alternar.
- ✋ **Arrastar para reordenar** (toque no ícone ≡ e arraste para cima/baixo).
- 🗓️ **Google Calendar:** tarefas com data/hora viram eventos no seu calendário.
- ⏰ **Alarme:** definido como lembrete do evento; **quem dispara o alarme é o app Google Calendar**, nativamente no Android (popup). A cor da prioridade vira a cor do evento no Calendar (Tomate/Banana/Manjericão).

### Arquivos do projeto
| Arquivo | Função |
|---------|--------|
| `index.html` | O app inteiro (UI + lógica + Google Calendar). |
| `manifest.webmanifest` | Faz o Android tratar como app instalável. |
| `sw.js` | Service worker — funciona offline. |
| `icon-192.png`, `icon-512.png` | Ícones do app. |

---

## 7. Como colocar para funcionar (passo a passo)

> Por que precisa de tudo isso? O login do Google **exige HTTPS** e que a tarefa rode num "endereço" autorizado. Por isso publicamos num host gratuito (GitHub Pages) em vez de abrir o arquivo direto.

### Parte 1 — Publicar o app (GitHub Pages, grátis e com HTTPS)
1. Crie uma conta em https://github.com (se não tiver).
2. Crie um repositório novo (ex.: `minhas-tarefas`), público.
3. Suba os arquivos: `index.html`, `manifest.webmanifest`, `sw.js`, `icon-192.png`, `icon-512.png`.
4. No repositório: **Settings → Pages → Source: "Deploy from a branch" → Branch: `main` / `(root)` → Save**.
5. Aguarde ~1 min. O GitHub mostra o endereço, algo como: `https://SEU_USUARIO.github.io/minhas-tarefas/`. **Guarde esse endereço.**

### Parte 2 — Criar o acesso ao Google Calendar (OAuth Client ID)
1. Acesse https://console.cloud.google.com e faça login.
2. Crie um projeto (menu no topo → "Novo projeto" → dê um nome → Criar).
3. Ative a API: menu **APIs e serviços → Biblioteca →** procure **"Google Calendar API" → Ativar**.
4. Configure a tela de consentimento: **APIs e serviços → Tela de permissão OAuth**.
   - Tipo de usuário: **Externo** → Criar.
   - Preencha nome do app, seu e-mail de suporte e de contato. Salvar.
   - Em **Usuários de teste**, adicione seu próprio e-mail do Google (`eduardoermakoff@gmail.com`). *(Sem isso o login é bloqueado.)*
5. Crie a credencial: **APIs e serviços → Credenciais → Criar credenciais → ID do cliente OAuth**.
   - Tipo de aplicativo: **Aplicativo da Web**.
   - Em **Origens JavaScript autorizadas**, adicione o **endereço da Parte 1** (ex.: `https://SEU_USUARIO.github.io`). Use só o domínio, sem a barra final nem o subcaminho.
   - Criar → copie o **Client ID** (termina em `...apps.googleusercontent.com`).

### Parte 3 — Conectar e instalar no Android
1. Abra o endereço do app no **Chrome do Android**.
2. Toque em **⚙️ Configuração** e cole o **Client ID**. (Fica salvo no aparelho.)
3. Toque em **Conectar Google** → faça login → autorize o acesso ao calendário.
4. **Instalar como app:** menu do Chrome (⋮) → **"Adicionar à tela inicial" / "Instalar app"**.
5. Crie uma tarefa com data/hora → ela aparece no seu Google Calendar e o **alarme tocará** no horário do lembrete.

### Dica de teste no PC (antes de publicar)
- O login do Google **não funciona** abrindo o arquivo direto (`file://`). Para testar no PC, sirva por `http://localhost` (ex.: `python -m http.server` na pasta) e adicione `http://localhost:8000` nas "Origens JavaScript autorizadas". As tarefas, cores e arrastar funcionam mesmo sem login.

---

## 8. Limitações conhecidas (v1) e próximos passos
- Não há edição inline do texto/data depois de criada (dá para apagar e recriar). Pode ser adicionada depois.
- O token de login do Google dura ~1h; o app pede novamente quando necessário.
- Concluir (✓) uma tarefa **remove o evento/alarme** do Calendar (comportamento proposital).
- Possíveis evoluções: editar tarefas, subtarefas, datas recorrentes, empacotar como `.apk` via PWABuilder.

## 9. Histórico de decisões
- **2026-06-07:** Plataforma Android, PWA. App escolhido: Lista de Tarefas com Google Calendar (OAuth completo), arrastar-para-reordenar e prioridades coloridas. App implementado em arquivo único + manifest + service worker + ícones.
