# Segurança e sanitização

O pacote original do Power Automate **não faz parte deste repositório público**.

## Informações removidas

A versão pública não contém valores reais de:

- Tenant ID;
- User ID;
- endereço de e-mail corporativo;
- Group ID;
- Plan ID;
- Bucket ID;
- Connection ID;
- nomes ou procedimentos internos desnecessários;
- descrições operacionais específicas do ambiente de origem.

## Por que o export bruto não é publicado?

Um pacote exportado pode incluir metadados suficientes para revelar a estrutura do ambiente Microsoft 365, mesmo quando não contém uma senha ou token utilizável. Esses dados não são necessários para demonstrar a lógica do projeto.

## Arquivos e screenshots

Antes de publicar uma captura de tela, revise:

- barra de endereço;
- e-mail do usuário;
- nome da organização;
- nomes de grupos e planos;
- IDs presentes em URLs;
- nomes internos de locais/equipamentos;
- detalhes operacionais;
- notificações visíveis na interface.

Quando necessário, recorte ou oculte essas informações antes do commit.

## Exemplos públicos

Use placeholders como:

```text
<TENANT_ID>
<GROUP_ID>
<PLAN_ID>
<BUCKET_ID>
<ASSIGNEE_EMAIL>
<DATA_BASE>
<FUSO_HORARIO>
```

O objetivo é tornar a solução compreensível e reproduzível sem publicar informações do ambiente de origem.
