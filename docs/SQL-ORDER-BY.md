# ORDER BY

Para determinar a ordem das linhas em um `result set` acrescentamos a clausula `ORDER BY` a um `SELECT`. Ela é escrita depois da clausula `FROM`.

```sql
SELECT
   lista_de_colunas
FROM
   tabela
ORDER BY
    coluna1 ASC,
    coluna2 DESC;
```


- `ASC` significa que `coluna1` vai ser usada para ordenar as linhas do result set e os valores da `coluna1` ficarão em ordem `Ascendente`.

- `DESC` é igual, mas os valores de `coluna2` ficarão em ordem `Descendente`.

Se não for incluído `ASC` ou `DESC` o padrão é `ASC`.

## Exemplos

Para ordenar por `AlbumId` uma consula à tabela `tracks`


```sql
SELECT
	name,
	milliseconds, 
	albumid
FROM
	tracks
ORDER BY
	albumid ASC;
```

Para ordenar o `result set` dessa consulta também por `milliseconds`

```sql
SELECT
	name,
	milliseconds, 
	albumid
FROM
	tracks
ORDER BY
	albumid ASC,
    milliseconds DESC;
```

Nesse caso, a resposta é ordenada primeiro por `albumid` e, para cada `albumid`, as linhas correspondentes a esse `albumid` serão ordenadas por `milliseconds`.

## Ordenação dos NULLs

O valor `NULL` em banco de dados significa que um valor está faltando ou que nenhum valor se aplica a um certo registro na tabela.

Por exemplo, se estamos cadastrando um contato e não temos o seu email, poderíamos resolver isso colocando no seu registro um email obviamente falso, como `sem@email`. Ou, poderíamos deixar uma string em branco. Mas, é para isso que existe o `NULL`.