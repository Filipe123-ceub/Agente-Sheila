# Tarefas de Implementação — Agente Sheila

## Fase 1 — Fundação do Projeto

- [ ] T01 — Configurar instância do n8n (Cloud ou local) e validar acesso.
- [ ] T02 — Criar credencial do WhatsApp Business API (Z-API, Evolution
      API, 360dialog ou Twilio — definir provedor).
- [ ] T03 — Criar credencial do Google Calendar (OAuth2) no n8n.
- [ ] T04 — Configurar credencial de IA (GPT-4o) no AI Agent node.
- [ ] T05 — Criar planilha/banco para log de conversas e captação de
      leads de outras áreas (Google Sheets ou tabela do banco disponível).

## Fase 2 — Entrada de Leads

- [ ] T06 — Configurar Webhook/trigger de recebimento de leads do Meta
      Lead Ads.
- [ ] T07 — Testar recebimento de um lead de exemplo (payload simulado)
      e validar que os dados (nome, telefone) chegam corretamente.
- [ ] T08 — Disparar mensagem inicial de apresentação da Sheila via
      WhatsApp ao receber um novo lead.

## Fase 3 — Conversa e Coleta de Dados

- [ ] T09 — Escrever o system prompt da Sheila (persona, tom, limites —
      baseado na `constitution.md`).
- [ ] T10 — Implementar o AI Agent node com o prompt da Sheila.
- [ ] T11 — Definir e implementar o roteiro de perguntas mínimas: idade,
      tempo de contribuição, tipo de contribuição, pretensão do lead.
- [ ] T12 — Implementar guardrail de bloqueio de menção a preços
      (validação pós-resposta da IA).
- [ ] T13 — Implementar guardrail de "postura de atendente" (bloquear
      respostas que soem como consultoria jurídica/identificação como
      advogada).

## Fase 4 — Motor de Elegibilidade

- [ ] T14 — Levantar as regras vigentes de aposentadoria (idade mínima,
      tempo de contribuição, regras de transição da reforma
      previdenciária).
- [ ] T15 — Implementar Code Node com a lógica determinística de
      elegibilidade (entrada: dados do lead; saída: qualificado
      true/false + regra aplicável).
- [ ] T16 — Testar o motor de regras com casos variados (qualificado por
      idade, por tempo de contribuição, por regra de transição, não
      qualificado).

## Fase 5 — Roteamento

- [ ] T17 — Implementar Switch Node com as 3 rotas: qualificado →
      agendamento; outra área de direito → parceiros; nenhum dos dois →
      encerramento cordial.
- [ ] T18 — Implementar identificação de "outra área de direito"
      mencionada pelo lead (trabalhista, família, consumidor, etc.).

## Fase 6 — Agendamento

- [ ] T19 — Implementar consulta de disponibilidade (`freeBusy.query`)
      restrita a seg-qui, 09h-17h.
- [ ] T20 — Implementar oferta de até 3 horários livres ao lead.
- [ ] T21 — Implementar criação do evento no Google Calendar com dados do
      lead + resumo da conversa na descrição.
- [ ] T22 — Testar tentativa de agendamento fora da janela permitida
      (deve ser bloqueada/não oferecida).

## Fase 7 — Notificação ao Dr. Danilo

- [ ] T23 — Implementar prompt de resumo de conversa (curto, sem opinião
      jurídica).
- [ ] T24 — Implementar envio de notificação via WhatsApp ao Dr. Danilo
      após agendamento confirmado.
- [ ] T25 — Implementar envio de notificação via WhatsApp ao Dr. Danilo
      quando identificada outra área de direito (sem descarte do lead).

## Fase 8 — Captação para Parceiros

- [ ] T26 — Implementar registro estruturado (planilha/banco) dos leads
      de outras áreas, com dados + área de interesse.

## Fase 9 — Testes e Validação (Edge Cases)

- [ ] T27 — Testar: lead pede preço → Sheila não informa.
- [ ] T28 — Testar: lead pergunta diretamente "já tenho direito?" → Sheila
      não dá parecer definitivo, apenas coleta dados.
- [ ] T29 — Testar: lead menciona direito trabalhista → não é descartado,
      Dr. Danilo é notificado.
- [ ] T30 — Testar: lead tenta agendar sexta-feira ou fora de 09h-17h →
      sistema não oferece o horário.
- [ ] T31 — Revisar logs de todas as conversas de teste para auditoria.

## Fase 10 — Documentação e Apresentação

- [ ] T32 — Finalizar `README.md` do repositório com visão geral,
      arquitetura, stack e guardrails.
- [ ] T33 — Preparar demonstração (roteiro de teste ao vivo ou vídeo) para
      a banca avaliadora do hackathon.