# SELECT - WHERE

Usamos a cláusula `WHERE` para filtrar linhas de um `result set` retornado por uma pesquisa com `SELECT`.


A cláusula `WHERE` é opcional no comando `SELECT`. Ela vai depois do `FROM` e tem a seguinte sintaxe:

```sql
SELECT
	colunas
FROM
	tabela
WHERE
	condição;
```


Apesar da ordem em que são escritas, as cláusulas não são avaliadas pelo SQLite na mesma sequência. A ordem de avaliação é a seguinte:

1. É verificada a `tabela` que está na cláusula `FROM`
2. São avaliadas as condições da cláusula `WHERE` e as linhas que satisfazem essas condições são lidas da `tabela`
3. O `result set` é montado com base nas linhas do passo 2 e nas colunas da cláusula `SELECT`

A sintaxe da condição de pesquisa da cláusula `WHERE` é a seguinte:

```sql
expressão_da_esquerda OPERADOR_DE_COMPARAÇÃO expressão_da_direita
```


## Operadores de comparação

Os `operadores de comparação` são binários (precisam de dois operandos) e seguem a notação `infixa` (são escritos entre cada um dos operandos).


Em SQL os operadores de comparação são:

|Operador|Significado|
|--------|-----------|
|=| igual a|
|<> ou !=| diferente de|
|<|menor que|
|>|maior que|
|<=| menor ou igual a|
|>=| maior ou igual a|

## Operadores lógicos

Usamos os operadores lógicos para testar o `valor de verdade` de expressões construídas com os operadores de comparação. 

Estes operadores retornam 0 ou 1, dependendo se a resposta for `true` (1) ou `false` (0).

|Operador|Significado|
|-|-|
LIKE|Compara padrões. Retorna 1 se um valor casar com o padrão informado.
BETWEEN|Testa um intervalo. Se um valor está dentro do intervalo, retorna 1
AND|Retorna 1 se as duas expressões forem 1 e 0 se alguma delas for 0
OR|Retorna 1 se qualquer uma das expressões for 1.
IN|Retorna 1 se um valor petence a uma certa lista de valores informada como parâmetro para `IN`
EXISTS|Retorna 1 se uma subconsulta contém linhas (ou seja, se o valor existe)
NOT|Inverte/nega o valor de verdade de um operador.
ANY|Retorna 1 se alguma das expressões for 1
ALL|Retorna 1 se todas as expressões forem 1

## Exemplos com WHERE

Exemplos:

```sql
WHERE coluna1 = 100;

WHERE coluna2 IN (1,2,3);

WHERE coluna3 LIKE 'An%';

WHERE coluna4 BETWEEN 10 AND 20;
```

> Também vamos usar a cláusula `WHERE` com os comandos `UPDATE` e `DELETE` mais adiante.

