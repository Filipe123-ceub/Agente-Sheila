# Plano Técnico — Agente Sheila

## 1. Arquitetura Geral

Meta Ads (Lead Ads)
│
▼
Webhook Inbound (n8n)
│
▼
AI Agent "Sheila" (n8n + GPT-4o)
├─ Checa requisitos vigentes do INSS (regra determinística)
└─ Aplica guardrails (sem preço / sem consultoria jurídica)
│
├── Lead Qualificado ──► Google Calendar (Agendamento)
│ │
│ ▼
│ Notificação via WhatsApp
│ para o Dr. Danilo Duarte
│
└── Outras Áreas ──► Captação para Parceiros
(Dados + Resumo)
│
▼
Notificação via WhatsApp
para o Dr. Danilo Duarte


## 2. Componentes e Responsabilidades

### 2.1 Entrada de Leads
- **Origem**: Meta Lead Ads.
- **Mecanismo**: Webhook n8n recebe o payload do lead (nome, telefone,
  campanha de origem).
- **Ação**: dispara a primeira mensagem via WhatsApp Business API,
  apresentando a Sheila.

### 2.2 AI Agent "Sheila" (n8n + GPT-4o)
- **Persona**: definida via system prompt (ver `docs/prompt-sheila.md`,
  a criar), reforçando os princípios da `constitution.md`.
- **Responsabilidades**:
  - Conduzir a conversa coletando: idade, tempo de contribuição, tipo de
    contribuição (CLT, autônomo, rural, etc.), pretensão do lead.
  - Nunca mencionar valores.
  - Nunca se identificar como advogada.
  - Identificar se o lead menciona necessidade de outra área do direito.

### 2.3 Motor de Regras de Elegibilidade (determinístico)
- Implementado como **Code Node** (JavaScript) dentro do n8n, não
  delegado à LLM.
- Entradas: idade, tempo de contribuição, gênero (quando aplicável às
  regras de transição), tipo de segurado.
- Saída: `qualificado: true/false` + `regra_aplicavel` (ex: idade
  mínima progressiva, pedágio 50%, pedágio 100%, tempo de contribuição).
- Racional: regras previdenciárias são objetivas — não devem depender de
  interpretação subjetiva da IA, reduzindo risco de erro.

### 2.4 Roteamento (Switch Node)
- `qualificado = true` → fluxo de agendamento.
- `qualificado = false` mas `outra_area_direito = true` → fluxo de
  captação para parceiros (sem descartar o lead).
- `qualificado = false` e sem outra área identificada → encerramento
  cordial, sem descarte de dado (mantém registro para eventual
  contato futuro).

### 2.5 Agendamento (Google Calendar)
- **Node**: Google Calendar (credencial OAuth2).
- **Regra de disponibilidade**: `freeBusy.query` restrito a:
  - Dias: segunda a quinta-feira.
  - Horário: 09h às 17h.
- **Fluxo**: gerar até 3 opções de horário livre → lead escolhe →
  criar evento com dados do lead + resumo da conversa na descrição.

### 2.6 Notificação ao Dr. Danilo
- **Canal**: WhatsApp Business API (mesmo provedor da entrada).
- **Gatilho**: após confirmação de agendamento OU identificação de
  lead de outra área.
- **Conteúdo da mensagem**:
  - Nome e telefone do lead.
  - Tipo de direito identificado (previdenciário / outra área).
  - Resumo da conversa (gerado por um segundo prompt de IA, focado só
    em resumir, sem opinião jurídica).
  - Horário agendado (se aplicável).

### 2.7 Captação para Parceiros
- Registro simples (ex: Google Sheets ou banco de dados) com os dados do
  lead + área de interesse, para consulta posterior do Dr. Danilo sobre
  encaminhamento a advogado parceiro.

## 3. Guardrails Técnicos (além do prompt)

| Guardrail | Implementação |
|---|---|
| Sem menção a preço | Validação pós-resposta da IA (regex/filtro) antes de enviar ao lead |
| Sem postura de advogada | Reforço no system prompt + revisão de respostas por regex de palavras-chave ("aconselho", "recomendo judicialmente", etc.) |
| Elegibilidade correta | Lógica determinística (Code Node), não deixada só para a LLM |
| Agendamento fora da janela | Validação de dia/horário antes de expor slots ao lead |

## 4. Stack Técnica
- **Orquestração**: n8n
- **IA conversacional**: GPT-4o (via AI Agent node)
- **Canal de mensagens**: WhatsApp Business API
- **Agendamento**: Google Calendar API
- **Registro/Auditoria**: Google Sheets (ou banco relacional, a definir)
- **Origem de leads**: Meta Ads (Lead Ads)

## 5. Fluxo de Dados (resumo)
1. Lead preenche anúncio → Webhook recebe payload.
2. Sheila inicia conversa via WhatsApp.
3. Dados coletados → Code Node avalia elegibilidade.
4. Switch decide rota (agendamento / parceiros / encerramento).
5. Se agendamento: Google Calendar cria evento.
6. Notificação final ao Dr. Danilo via WhatsApp.
7. Log da conversa e do resultado é registrado para auditoria.