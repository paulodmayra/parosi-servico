# Parosi Serviços — Documentação Consolidada do Projeto & Guia para Futuros Agentes

> **Versão Ativa:** `v1.4.2`  
> **Status de Deploy:** Ativo na Vercel — [https://parosi-servico.vercel.app](https://parosi-servico.vercel.app)  
> **Ambiente Local:** `http://localhost:8088` (Servidor Python `http.server`)  
> **Última Atualização:** 12 de Agosto de 2026

---

## 🏛️ 1. Visão Geral do Projeto

A landing page **Parosi Serviços** é uma plataforma web híbrida de alta conversão projetada para apresentar os dois pilares estratégicos da empresa:
1. **Serralheria & Estruturas Metálicas** (Manutenção geral, reparos em portões na região de São Carlos - SP, soldas TIG/Inox/Alumínio, toldos e fachadas).
2. **Automação Web & Soluções em IA** (Desenvolvimento de sistemas SaaS sob medida, automação de processos, criação de landing pages e ecossistema de robôs autônomos).

A aplicação utiliza arquitetura estática leve e responsiva (HTML5, Tailwind CSS via CDN e Vanilla JavaScript), com foco em **Glassmorphism**, estética escura industrial e alto desempenho de carregamento.

---

## 🎨 2. Design System & Identidade Visual

- **Paleta de Cores**:
  - **Fundo Primário**: `#121212` (`bg-iron`) com textura e gradientes sutis.
  - **Superfície de Cards**: `#1A1A1A` (`bg-charcoal`) com opacidade e efeito de vidro (`backdrop-blur-md`).
  - **Cor de Destaque / Accent**: Laranja Âmbar (`#FF6B00` / `hover:border-[#FF6B00]`).
  - **Tipografia**: Cores claras (`#E5E5E5`) com alto contraste para leitura em telas escuras.

- **Estilo de Cards e Efeitos Hover**:
  - Bordas afiadas (`sharp-border`) retas de 1px.
  - Quando o cursor passa por cima (hover), os cards acendem uma borda em laranja âmbar (`#FF6B00`), destacando o texto e os títulos.
  - **Alinhamento Geométrico**:
    - **Coluna Esquerda (Serralheria)**: Os cards de *Manutenção Geral*, *Soldas Especiais* e *Toldos e Fachadas* estão alinhados com o topo do título "SERRALHERIA".
    - **Coluna Direita (Automação Web)**: Os cards de *Soluções Customizadas*, *Landing Pages* e *Agentes de I.A.* estão simetricamente alinhados com o topo do título "AUTOMAÇÃO WEB".

---

## 🧩 3. Mapeamento de Modais & Funcionalidades Interativas

Todos os modais compartilham uma estrutura padronizada de glassmorphism, suporte ao botão `[X]` no canto superior direito, fecho por overlay e suporte à tecla `ESC`.

| ID do Modal | Nome / Função | Conteúdo Principal | Ação / CTA |
| :--- | :--- | :--- | :--- |
| `#modal-serralheria` | Manutenção Geral & Portões | Reparos em portões, soldas e estrutura metálica na região de São Carlos - SP. | Botão WhatsApp (+55 16 98875-7849) |
| `#modal-soldas` | Soldas Especiais | Soldas TIG, Inox e Alumínio com fino acabamento anti-oxidação. | Botão WhatsApp (+55 16 98875-7849) |
| `#modal-toldos` | Toldos e Fachadas | Coberturas de policarbonato, fachadas em ACM e letras caixa iluminadas. | Botão WhatsApp (+55 16 98875-7849) |
| `#modal-solucoes` | Soluções Customizadas *(Novo)* | Desenvolvimento de sistemas SaaS sob medida e automação de fluxos operacionais/logísticos. | Botão de Orçamento no WhatsApp |
| `#modal-landing` | Arsenal de Landing Pages | Carrossel responsivo (`snap-x`) exibindo 9 capturas reais de portfólio. | Botão WhatsApp (+55 16 98875-7849) |
| `#modal-agentes` | Agentes de I.A. Autônomos | Robôs cognitivos para atendimento SDR (Vendas B2B), CS Retention e Auditoria. | Botão de Orçamento no WhatsApp |

---

## 📱 4. Integração de Canais, Redes Sociais & Atendimento

1. **WhatsApp de Atendimento**:
   - **Número**: `+55 16 98875-7849` (Paulo Roberto Silva).
   - Todos os botões de CTA dos modais geram links diretos com mensagens pré-formatadas para o WhatsApp.

2. **Telegram & Bot**:
   - Ícone do Telegram no rodapé direcionando para o canal de suporte/atendimento.
   - **Nota Técnica sobre Navegadores Linux**: Quando o usuário clica no link do Telegram via navegador Linux, a mensagem do sistema `xdg-open` solicita autorização para abrir o aplicativo do Telegram (ou abrir a versão web). Esse comportamento é uma medida de segurança padrão do ecossistema e não representa um erro.

3. **Redes Sociais & Contato no Rodapé**:
   - Ícones de Instagram (`/parosiservicos`), WhatsApp e Telegram centralizados com efeito hover acendendo em laranja âmbar.
   - Indicação visual da versão do sistema: **`v1.4.2`**.

4. **Rodapé e Links de Referência**:
   - Marca da empresa: **PAROSI SERVIÇO - PAULO ROBERTO SILVA** + Endereço físico em São Carlos - SP.
   - Crédito de Engenharia: **ENGENHARIA DE CONVERSÃO POR QUANTUM OMEGA**. Ao passar o mouse sobre o texto, o link indica dinamicamente o endereço do site em produção (`https://parosi-servico.vercel.app`).

---

## ⚙️ 5. Instruções para Execução Local & Manutenção

### Como Rodar o Projeto Localmente:
```bash
cd /home/paulo/Downloads/pasta-teste-modelos-kit2/parosi_servico
python3 -m http.server 8088
```
Acesse `http://localhost:8088` em qualquer navegador.

### Como Realizar o Deploy na Vercel (via Git):
```bash
git add .
git commit -m "feat: descreva as alterações realizadas"
git push origin master
```
A Vercel sincronizará automaticamente a branch `master` e publicará a nova versão em poucos segundos.

---

## 📌 6. Orientações Finais para Agentes Futuros

- **Preservação de Estilo**: Mantenha a paleta escuro/âmbar, bordas retas (`sharp-border`), e transições suaves de hover (`transition-all duration-300`).
- **Adição de Imagens**: Quaisquer novas capturas de tela ou fotos de serviços devem ser colocadas na pasta `imagens/portfolio/` ou `imagens/` para manter a integridade dos caminhos de mídia.
- **Integridade da Versão**: Lembre-se de incrementar a string de versão no rodapé (ex: `v1.4.3`) a cada novo ciclo de entrega de melhorias visuais ou funcionais.

---
*Documento homologado e registrado no repositório oficial da Parosi Serviços.*
