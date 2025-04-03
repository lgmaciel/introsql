# Exercícios

1. Abra o banco de dados `chinook.db` no `SQLite`
1. Liste todas as tabelas do banco
1. Mude o modo de exibição dos resultados para `table` usando o comando
    ```
    .mode table
    ```
1. Mostre a estrutura da tabela `tracks` usando a função `table_info()`, assim:
    ```sql
    PRAGMA table_info(tracks);
    ```
1. Use o comando `SELECT` para consultar as colunas `composer`, `unitprice` e `milliseconds` da tabela tracks.
1. Agora, vamos gerar um relatório contendo todas as faixas disponíveis na loja onde devem constar o título da faixa, o compositor e o preço dela.

    Primeiro, Escreva uma consulta usando o comando `SELECT` que liste os valores das colunas `name`, `composer` e `unitprice`.
    
    Depois que estiver satisfeito com a consulta, gere o relatório. Para isso, mude a saída padrão para que ela seja redirecionada para um arquivo chamado `relatorio.txt`
```
.output 'relatorio.txt'
```
Agora, rode de novo a consulta que você fez. Confira no seu diretório de trabalho se lá existe um arquivo chamado `relatorio.txt` com os dados da consulta.                                                                                                     