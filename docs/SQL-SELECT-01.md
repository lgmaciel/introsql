# Consultas simples

O comando `SELECT` é um dos mais usados do SQL. Na maioria das vezes usamos o `SELECT` para consultar dados de uma ou mais tabelas.

A sintaxe do `SELECT` é:

```sql
SELECT DISTINCT lista_de_colunas
FROM lista_de_tabelas
  JOIN table ON condição
WHERE critério_de_filtragem_das_linhas
ORDER BY coluna
LIMIT número_de_linhas OFFSET deslocamento
GROUP BY coluna
HAVING filtro_dos_grupos;
```

- `ORDER BY` ordena o conjunto de resultados
- `DISTINCT` consulta apenas linhas únicas na tabela (sem valore repetidos)
- `WHERE` filtra linhas do conjunto de resultados
- `LIMIT` `OFFSET` limitam o número de linhas retornadas
- `INNER JOIN` ou `LEFT JOIN` consultam dados de várias tabela usando `JOIN` (juntando colunas das tabelas)
- `GROUP BY` agrupa linhas em grupos e aplica funções de agregação para cada grupo
- `HAVING` filtra grupos

Começamos consultando dados que estão em uma só tabela.

```sql
SELECT lista_de_colunas
FROM tabela;
```

Por exemplo, para consultar a tabela `tracks` que tem informações sobre faixas de música disponíveis no banco `chinook.db`

```sql
SELECT name, composer, unitprice
FROM tracks;
```
