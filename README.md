# Planner Automation — Escala 12x36

Automação desenvolvida com **Microsoft Power Automate** e **Microsoft Planner** para criar tarefas recorrentes apenas nos dias correspondentes a uma escala **12x36**.

O projeto surgiu da necessidade de substituir a criação manual de tarefas operacionais por um fluxo capaz de calcular os dias corretos da escala, criar múltiplas tarefas com horários definidos, atribuí-las ao responsável e complementar cada tarefa com descrição e checklist.

> Este repositório contém documentação e exemplos **sanitizados**. IDs de tenant, grupo, plano, bucket, conexões, usuários e procedimentos internos do ambiente original não são publicados.

## O problema

Uma recorrência diária comum não representa corretamente uma escala 12x36. O fluxo precisa executar todos os dias para verificar a data atual, mas só deve criar tarefas quando o dia pertence à escala configurada.

## A solução

O fluxo usa uma **data-base** conhecida da escala e calcula a quantidade de dias transcorridos. Em seguida aplica módulo 2:

```text
(data_atual - data_base) em dias % 2
```

Quando o resultado é `0`, o fluxo considera o dia válido e cria as tarefas configuradas no Planner.

## Fluxo resumido

```text
Recorrência diária
       │
       ▼
Converter data para fuso local
       │
       ▼
Calcular diferença para DATA_BASE
       │
       ▼
        mod(dias, 2)
       │
   ┌───┴────┐
   │        │
   0       != 0
   │        │
   ▼        ▼
Criar      Encerrar
Tarefas    sem criar
   │
   ▼
Atualizar descrição e checklist
```

## Recursos demonstrados

- recorrência agendada no Power Automate;
- cálculo de escala 12x36 por expressão;
- tratamento explícito de fuso horário;
- criação automática de múltiplas tarefas no Planner;
- definição de vencimentos por horário;
- atribuição automática ao responsável;
- atualização dos detalhes da tarefa;
- inclusão automática de checklist;
- encadeamento de ações com `runAfter`;
- documentação segura de uma automação originalmente usada em ambiente corporativo.

## Tecnologias

- Microsoft Power Automate
- Microsoft Planner
- Microsoft 365
- Workflow Definition Language / expressões do Power Automate

## Documentação

- [`docs/architecture.md`](docs/architecture.md) — arquitetura e sequência do fluxo.
- [`docs/flow-logic.md`](docs/flow-logic.md) — cálculo da escala 12x36 e expressões.
- [`docs/setup.md`](docs/setup.md) — guia para reconstruir a automação em outro ambiente.
- [`docs/security.md`](docs/security.md) — informações removidas da versão pública e cuidados de publicação.
- [`examples/flow-example.json`](examples/flow-example.json) — representação sanitizada da lógica do fluxo.

## Screenshots

A pasta [`screenshots/`](screenshots/) está preparada para receber registros do fluxo e do resultado no Planner. Antes de publicar imagens, remova ou oculte e-mails, IDs, nomes internos, informações de tenant e dados operacionais que não devam ser públicos.

## Status

🟢 **Automação funcional e validada no ambiente de origem.**

A documentação pública foi criada separadamente do pacote exportado original para evitar exposição de identificadores e informações corporativas.

## Autor

**Bruno Ribeiro**  
Projeto integrante do portfólio **B.A Dev Lab**.

**Desenvolvimento • Automação • Tecnologia**
