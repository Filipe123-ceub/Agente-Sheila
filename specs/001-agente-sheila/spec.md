# Especificação — Agente Sheila

## Visão Geral
Assistente de IA (Sheila) que atua como primeira linha de atendimento via
WhatsApp para leads gerados por campanhas de Meta Ads sobre aposentadoria,
qualificando-os e agendando consultas no Google Agenda com o escritório do
Dr. Danilo Duarte.

## Problema
O escritório do Dr. Danilo investe em anúncios de aposentadoria no Meta
Ads. O alto volume de leads consome tempo excessivo da equipe em:
- Responder rapidamente todos os contatos do WhatsApp.
- Identificar manualmente quem já tem direito à aposentadoria.
- Decidir o que fazer com leads que buscam outras áreas do direito
  (hoje descartados sem direcionamento aos parceiros).

## Usuários e Stakeholders
- **Lead** — pessoa que preenche o anúncio no Meta Ads buscando informação
  sobre aposentadoria.
- **Dr. Danilo Duarte** — advogado responsável pela área previdenciária,
  recebe notificações e agenda confirmada.
- **Advogados parceiros** — atuam em outras áreas do direito; recebem
  encaminhamento quando aplicável.

## Requisitos Funcionais

1. **RF01** — O sistema deve receber leads oriundos do Meta Ads (Lead Ads).
2. **RF02** — A Sheila deve iniciar contato via WhatsApp e se apresentar
   como atendente (nunca como advogada).
3. **RF03** — A Sheila deve coletar dados suficientes para avaliar o
   direito à aposentadoria (idade, tempo de contribuição, tipo de
   trabalho/contribuição).
4. **RF04** — O sistema deve aplicar regras determinísticas da legislação
   previdenciária vigente para classificar o lead como qualificado ou não.
5. **RF05** — Se qualificado, a Sheila deve consultar horários livres no
   Google Agenda (seg-qui, 09h-17h) e oferecer opções ao lead.
6. **RF06** — Ao confirmar um horário, o sistema deve criar o evento no
   Google Agenda com os dados do lead.
7. **RF07** — Após o agendamento, o sistema deve notificar o Dr. Danilo via
   WhatsApp com: dados do lead + resumo da conversa + horário agendado.
8. **RF08** — Se o lead mencionar um direito de outra área (não
   previdenciário), o sistema não deve descartá-lo; deve notificar o
   Dr. Danilo para possível encaminhamento a um parceiro.
9. **RF09** — A Sheila nunca deve informar valores/preços em nenhuma etapa
   da conversa.

## Requisitos Não-Funcionais

- **RNF01** — Nenhum agendamento pode ocorrer fora da janela seg-qui,
  09h-17h.
- **RNF02** — Todas as conversas devem ser registradas (log) para fins de
  auditoria e avaliação do hackathon.
- **RNF03** — Guardrails de conteúdo (sem preço, sem postura de
  advogada) devem ser validados por lógica determinística além do
  prompt da IA, para reduzir risco de falha do modelo.

## Critérios de Qualificação (Direito à Aposentadoria)
> A ser detalhado com as regras vigentes (idade mínima + tempo de
> contribuição, regras de transição da reforma da previdência) — ver
> seção técnica em `plan.md`.

## Fora de Escopo
- Consultoria jurídica de qualquer tipo.
- Informar valores de honorários.
- Atendimento fora do canal WhatsApp.

## Casos de Teste Críticos (Edge Cases)
1. Lead pergunta o preço da consulta → Sheila não informa, redireciona.
2. Lead pergunta se "já tem direito" de forma direta → Sheila coleta dados,
   não dá parecer jurídico definitivo.
3. Lead menciona direito trabalhista → não descarta, notifica Dr. Danilo.
4. Lead tenta agendar fora do horário permitido → sistema não oferece esse
   horário.
5. Lead se identifica como já tendo processo em andamento com outro
   advogado → tratar com cautela (a definir fluxo).