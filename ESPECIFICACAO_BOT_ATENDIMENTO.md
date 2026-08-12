# Especificação Técnica & Guia de Deploy — Bot Telegram Parosi Serviços

> **Data:** 12 de Agosto de 2026  
> **Objetivo:** Documentar a arquitetura, o fluxo de "Falar com Atendente" e o procedimento de deploy do bot local para o servidor de produção.  
> **Público:** Desenvolvedor (Paulo) e Agentes de I.A. Futuros.

---

## 📌 1. Contexto & Circunstância do Projeto

1. **Desenvolvimento Local vs. Produção**:
   - O código-fonte do Bot do Telegram é versionado e mantido na máquina local em `/home/paulo/Downloads/pasta-teste-modelos-kit2/`.
   - O processo ativo do Bot em produção roda no **Servidor Remoto (VPS/Cloud)** do Paulo.

2. **Fluxo de Trabalho de Deploy (Local ➔ Servidor)**:
   - **Fase 1 (Desenvolvimento Local)**: Alteramos o código do bot na máquina local.
   - **Fase 2 (Validação Local)**: Testamos a lógica dos handlers, mensagens e botões.
   - **Fase 3 (Deploy no Servidor)**: Envia-se o código atualizado para o servidor (via `git push`, `rsync` ou `scp`) e reinicia-se o serviço do bot (`pm2 restart`, `systemctl restart bot` ou `docker restart`).

---

## 🛎️ 2. Arquitetura do Botão "📞 Falar com Atendente"

Quando um cliente clica na opção **"📞 Falar com Atendente"** após registrar um pedido (ex: Pedido `#51`), o bot executa a estratégia **Híbrida (Opção 1 + Opção 2 + Opção 4)**:

### A. Fluxo de Execução Técnica

```
[ Cliente clica em "Falar com Atendente" no Telegram ]
                        │
                        ├─► 1. Captura ID do Cliente, Nome, Pedido # e Data/Hora Exata (Timestamp)
                        │
                        ├─► 2. Envia Alerta Imediato para o TELEGRAM PRIVADO do Paulo
                        │
                        └─► 3. Exibe Sub-menu de Ações no Chat do CLIENTE:
                              ├── 🟢 Botão: "Chamar no WhatsApp do Paulo" (Link direto com mensagem pré-formatada)
                              ├── 📷 Botão: "Enviar Fotos ou Medidas do Projeto"
                              └── 📍 Botão: "Ver Endereço da Oficina (São Carlos - SP)"
```

---

## 📲 3. Detalhamento das Mensagens & Notificações

### 1. Mensagem Enviada ao Atendente (Paulo) no Telegram Privado:
```markdown
🚨 SOLICITAÇÃO DE ATENDIMENTO HUMANO
━━━━━━━━━━━━━━━━━━━━━━━━━
👤 Cliente: João Silva (@joaosilva)
🆔 ID Telegram: 987654321
📦 Pedido: #51 (Desenvolvimento Web / Serralheria)
⏱️ Horário do Contato: 12/08/2026 às 08:31:45
━━━━━━━━━━━━━━━━━━━━━━━━━
💡 O cliente recebeu o link do seu WhatsApp e o sub-menu de opções rápidas.
```

### 2. Mensagem & Sub-menu Exibidos para o Cliente:
```markdown
✅ Solicitação Registrada com Sucesso!

Notificamos o Paulo sobre o seu Pedido #51 registrado às 08:31.

Como prefere dar prosseguimento ao seu atendimento?
```
**Botões Inline Interativos:**
- `[ 🟢 Chamar no WhatsApp do Paulo ]` ➔ Redireciona para `https://wa.me/5516988757849?text=Olá%20Paulo,%20solicitei%20atendimento%20no%20Telegram%20para%20o%20Pedido%20%2351%20às%2008:31.`
- `[ 📷 Enviar Fotos ou Medidas ]` ➔ Prepara a caixa de mensagem para receber fotos do serviço.
- `[ 📍 Localização da Oficina ]` ➔ Envia a localização da oficina em São Carlos - SP.

---

## 💻 4. Estrutura de Código Recomendada (Python - `python-telegram-bot`)

```python
import logging
from datetime import datetime
from telegram import InlineKeyboardButton, InlineKeyboardMarkup, Update
from telegram.ext import ContextTypes

# ID do Telegram privado do Paulo para receber os alertas
PAULO_CHAT_ID = "SEU_TELEGRAM_CHAT_ID"

async def handler_falar_com_atendente(update: Update, context: ContextTypes.DEFAULT_TYPE):
    query = update.callback_query
    await query.answer()

    # 1. Obter informações do cliente e horário atual
    user = query.from_user
    cliente_nome = user.full_name
    cliente_user = f"@{user.username}" if user.username else "Sem @username"
    horario_completo = datetime.now().strftime("%d/%m/%Y às %H:%M:%S")
    horario_curto = datetime.now().strftime("%H:%M")
    pedido_num = context.user_data.get("ultimo_pedido", "#51")

    # 2. Notificar o Atendente (Paulo)
    alerta_atendente = (
        f"🚨 *SOLICITAÇÃO DE ATENDIMENTO HUMANO*\n"
        f"━━━━━━━━━━━━━━━━━━━━━━━━━\n"
        f"👤 *Cliente:* {cliente_nome} ({cliente_user})\n"
        f"🆔 *ID:* `{user.id}`\n"
        f"📦 *Pedido:* `{pedido_num}`\n"
        f"⏱️ *Horário:* `{horario_completo}`\n"
        f"━━━━━━━━━━━━━━━━━━━━━━━━━"
    )
    
    try:
        await context.bot.send_message(
            chat_id=PAULO_CHAT_ID,
            text=alerta_atendente,
            parse_mode="Markdown"
        )
    except Exception as e:
        logging.error(f"Erro ao notificar atendente: {e}")

    # 3. Criar Sub-menu para o Cliente
    msg_wa = f"Olá Paulo! Solicitei atendimento no Telegram para o Pedido {pedido_num} às {horario_curto}."
    url_wa = f"https://wa.me/5516988757849?text={msg_wa.replace(' ', '%20')}"

    keyboard = [
        [InlineKeyboardButton("🟢 Chamar no WhatsApp do Paulo", url=url_wa)],
        [InlineKeyboardButton("📷 Enviar Fotos ou Medidas", callback_data="btn_enviar_fotos")],
        [InlineKeyboardButton("📍 Localização da Oficina (São Carlos)", callback_data="btn_localizacao")]
    ]
    reply_markup = InlineKeyboardMarkup(keyboard)

    await query.edit_message_text(
        text=(
            f"✅ *Solicitação Registrada com Sucesso!*\n\n"
            f"Notificamos o Paulo sobre o seu *Pedido {pedido_num}* registrado às *{horario_curto}*.\n\n"
            f"Como prefere dar prosseguimento ao atendimento?"
        ),
        reply_markup=reply_markup,
        parse_mode="Markdown"
    )
```

---

## 🚀 5. Procedimento de Deploy (Passo a Passo)

1. **Desenvolvimento & Testes Locais**:
   - Editar o arquivo do bot na máquina local.
   - Executar o bot localmente em modo de teste para validar os botões e callbacks.

2. **Envio para o Repositório / Servidor**:
   ```bash
   git add .
   git commit -m "feat: implementa transbordo de atendimento humano com alerta de horario e whatsapp"
   git push origin master
   ```

3. **Atualização no Servidor Remoto**:
   - Conectar ao servidor via SSH:
     ```bash
     ssh usuario@seu-servidor.com
     ```
   - Puxar as alterações e reiniciar o serviço:
     ```bash
     cd /caminho/do/projeto/bot
     git pull origin master
     pm2 restart bot-telegram # ou systemctl restart telegram-bot
     ```

---
*Este documento deve ser consultado por agentes futuros antes de alterar qualquer manipulador (handler) ou fluxo de comunicação no bot do Telegram.*
