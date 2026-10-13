# Agente Sheila

Agente de IA para triagem automatizada de leads de aposentadoria, desenvolvido para o escritório de advocacia do **Dr. Danilo Duarte**, como parte do desafio **Polimatas Hackathon n8n**.

## 📋 Sobre o projeto

O Dr. Danilo Duarte é advogado especializado em aposentadoria e anuncia seus serviços no Meta Ads. O alto volume de leads gerados consome muito tempo da equipe do escritório na triagem manual — a Sheila existe para automatizar essa etapa.

**Sheila** atua como atendente virtual, conversando com os leads, qualificando-os e agendando a consulta diretamente na agenda do escritório, sem necessidade de intervenção humana até o momento do atendimento jurídico.

## 🎯 Objetivos do agente

- Fazer a triagem de leads vindos dos anúncios de aposentadoria
- Identificar se o lead é **qualificado**: busca a aposentadoria e já tem direito a se aposentar pela regra vigente
- Agendar a consulta automaticamente no **Google Agenda**, respeitando os horários livres de **segunda a quinta, das 09h às 17h**
- Notificar o advogado (via número de WhatsApp) após o agendamento, com os dados do lead e um resumo da conversa
- Identificar quando o lead busca uma área do direito fora da especialidade do escritório (mas atendida por parceiros) e informar o Dr. Danilo, em vez de descartar o lead

## 🚧 Restrições importantes

- A Sheila **não presta consultoria jurídica** e **não se apresenta como advogada**
- A Sheila **não informa preços** dos serviços
- Seu papel é estritamente de **atendente**, focado em qualificação e agendamento

## 🛠️ Stack

> _A preencher — descreva aqui as ferramentas usadas (ex: n8n, WhatsApp API, Google Calendar API, modelo de IA utilizado, etc.)_

## 🚀 Como rodar

> _A preencher com as instruções de instalação/execução do workflow n8n_

## 📂 Estrutura do projeto

```
.
├── .claude/skills/       # Skills utilizadas pelo agente
├── .specify/             # Configuração do fluxo Spec-Driven Development
└── specs/001-agente-sheila/  # Especificação, plano e tarefas da feature
```

## 📄 Contexto

Este projeto foi desenvolvido como parte do **Polimatas Hackathon n8n**.


