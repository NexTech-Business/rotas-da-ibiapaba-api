# Criando um Hook Local com pre-commit

O pre-commit é uma ferramenta fantástica não apenas para usar hooks criados pela comunidade, mas também para executar seus próprios scripts e comandos locais. Isso permite automatizar tarefas específicas do seu projeto, como rodar um linter customizado, verificar a formatação de um arquivo de configuração ou, como no exemplo anterior, atualizar o requirements.txt.

Esta documentação explica como adicionar um hook local ao seu fluxo de trabalho.

## Pré-requisitos

- O framework pre-commit deve estar instalado no seu ambiente (`pip install pre-commit`)
- O pre-commit já deve ter sido inicializado no seu repositório (`pre-commit install`)
- Um arquivo de configuração `.pre-commit-config.yaml` deve existir na raiz do seu projeto

## O Repositório local

Para criar um hook que executa um comando ou script do seu próprio projeto, você utiliza o repositório especial local. Dentro da sua configuração `.pre-commit-config.yaml`, você define uma entrada para `repo: local` e, em seguida, lista os hooks desejados.

### Anatomia de um Hook Local

A seguir estão os principais campos que você usará para definir seu hook:

- **id**: Um identificador único para o hook no seu arquivo de configuração
- **name**: Um nome amigável que será exibido no console quando o hook for executado
- **entry**: O comando que será executado. Este é o coração do seu hook
- **language**: Para hooks locais que executam comandos de sistema, o valor deve ser `system`. Isso diz ao pre-commit para usar os executáveis que já estão disponíveis no seu ambiente/PATH
- **stages** (Opcional): Define em qual estágio do Git o hook deve rodar. Os mais comuns são `commit`, `push`, `pre-push`. Se omitido, o padrão é rodar em todos os estágios
- **files** (Opcional): Uma expressão regular (regex) para filtrar em quais arquivos o hook deve ser aplicado. Isso é útil para rodar um linter apenas em arquivos de uma linguagem específica (ex: `\.py$`)
- **always_run** (Opcional): Se definido como true, o hook rodará mesmo que nenhum arquivo correspondente tenha sido modificado. O padrão é false

## Passo a Passo: Criando o Hook

1. Abra o arquivo `.pre-commit-config.yaml` no seu editor de código

2. Adicione ou edite a seção `repo: local`. Se você ainda não tem, adicione o seguinte bloco:

```yaml
repos:
  - repo: local
    hooks:
      # Seus hooks locais virão aqui
```

3. Defina seu novo hook. Adicione uma nova entrada sob hooks.

### Exemplo 1: Rodar o comando check do Django

Vamos criar um hook que roda `python manage.py check` para garantir que não há erros de configuração antes de cada commit.

```yaml
repos:
  - repo: local
    hooks:
      - id: django-check
        name: Executa o Django Check
        entry: python manage.py check
        language: system
        # Queremos que rode apenas em arquivos Python para evitar execuções desnecessárias
        files: \.py$
        stages: [pre-commit]
```

### Exemplo 2: Garantir que o requirements.txt seja atualizado

Este exemplo é um pouco mais complexo. Ele executa `pip freeze` e, se o requirements.txt for alterado, ele o adiciona ao commit.

```yaml
repos:
  - repo: local
    hooks:
      - id: requirements
        name: Atualiza requirements.txt
        # Usamos bash -c para executar múltiplos comandos
        entry: bash -c 'pip freeze > requirements.txt; git add requirements.txt'
        language: system
        # Este hook precisa rodar sempre para comparar o ambiente com o arquivo
        always_run: true
        # Passa os nomes dos arquivos como argumentos vazios para evitar problemas
        pass_filenames: false
        stages: [pre-commit]
```

> Se depois de salvar o arquivo `.pre-commit-config.yaml` e o comando não funcionar tente executar o comando `pre-commit install` no terminal.

## Como Funciona

Após salvar o arquivo `.pre-commit-config.yaml`, você não precisa fazer mais nada. Na próxima vez que você executar `git commit`, o pre-commit irá ler a configuração atualizada e executar seus novos hooks locais automaticamente. Se um hook falhar (ou seja, seu script sair com um código de erro diferente de zero), o processo de commit será interrompido, dando a você a chance de corrigir o problema.
