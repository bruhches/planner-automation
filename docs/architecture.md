# Arquitetura

## Visão geral

A automação é executada uma vez por dia. A recorrência diária funciona como um gatilho de verificação: a criação das tarefas só acontece quando a condição da escala 12x36 é satisfeita.

```text
Power Automate
│
├── Trigger: Recurrence
│
├── Condition: dia pertence à escala?
│   │
│   ├── Não → nenhuma tarefa é criada
│   │
│   └── Sim
│       ├── Criar tarefa 1
│       ├── Atualizar detalhes 1
│       ├── Criar tarefa 2
│       ├── Atualizar detalhes 2
│       ├── Criar tarefa 3
│       ├── Atualizar detalhes 3
│       ├── Criar tarefa 4
│       └── Atualizar detalhes 4
│
└── Microsoft Planner
```

## Componentes

### 1. Recorrência

O gatilho é configurado como recorrência diária, utilizando o fuso horário do ambiente em que a automação será executada.

### 2. Condição 12x36

A condição compara a data atual com uma data-base conhecida da escala. A diferença é convertida para dias e submetida a módulo 2.

### 3. Criação das tarefas

Nos dias válidos, o fluxo cria várias tarefas no Planner. Cada tarefa pode possuir:

- título;
- grupo e plano de destino;
- bucket;
- horário de vencimento;
- responsável.

Esses valores devem ser configurados com os identificadores do ambiente de destino e não devem ser copiados do ambiente original.

### 4. Atualização dos detalhes

Após cada criação, uma ação de atualização utiliza o ID retornado pelo Planner para adicionar descrição e checklist à tarefa recém-criada.

### 5. Encadeamento

As ações são sequenciais. A etapa seguinte só é executada após o sucesso da anterior, evitando que a atualização tente acessar uma tarefa ainda não criada.

## Separação entre lógica e ambiente

A lógica da escala é reutilizável. Os seguintes elementos são específicos de cada ambiente e devem ser configurados localmente:

- tenant;
- conexão do Planner;
- Microsoft 365 Group;
- Planner Plan;
- bucket;
- usuário responsável;
- títulos e horários;
- descrição e checklist.
