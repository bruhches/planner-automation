# Configuração

Este guia descreve como reconstruir a lógica em outro ambiente sem reutilizar identificadores do fluxo original.

## Pré-requisitos

- conta Microsoft com acesso ao Power Automate;
- acesso ao Microsoft Planner;
- um plano e bucket de destino;
- permissão para criar e atualizar tarefas nesse plano.

## 1. Criar o fluxo

No Power Automate, crie um **Scheduled cloud flow** com recorrência diária.

Configure o fuso horário correspondente ao ambiente onde as tarefas serão utilizadas.

## 2. Definir a data-base

Escolha uma data em que você sabe que a escala está ativa. Ela será `<DATA_BASE>` na expressão.

Exemplo conceitual:

```text
2026-01-01
```

Não copie uma data-base de outra escala sem verificar a paridade dos dias.

## 3. Adicionar a condição

Adicione uma ação **Condition** e use a expressão documentada em [`flow-logic.md`](flow-logic.md).

Compare o resultado com:

```text
0
```

## 4. Criar as tarefas

No ramo verdadeiro da condição, adicione ações **Planner — Create a task**.

Para cada tarefa configure:

- Group ID;
- Plan ID;
- título;
- Bucket ID;
- data/hora de vencimento;
- responsável.

Os IDs devem vir do seu próprio ambiente.

## 5. Adicionar detalhes

Após cada criação, adicione **Planner — Update task details**.

Utilize o ID retornado pela ação de criação correspondente. Configure uma descrição e, se necessário, um checklist apropriado ao seu processo.

Evite marcar verificações automaticamente como concluídas quando elas dependem de uma inspeção humana que ainda não ocorreu. O exemplo público usa itens genéricos apenas para demonstrar a estrutura.

## 6. Testar

Antes de ativar definitivamente:

1. use uma data-base cuja paridade você consiga validar;
2. execute um teste manual;
3. confirme se o ramo esperado foi escolhido;
4. verifique a criação no Planner;
5. valide responsável, vencimento, descrição e checklist;
6. teste também um dia que deveria cair no ramo falso.

## 7. Publicação no GitHub

Não publique o ZIP exportado diretamente sem auditá-lo. Exports do Power Automate podem conter identificadores do tenant, usuário, conexões e recursos do Microsoft 365.
