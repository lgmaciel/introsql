# GROUP BY

A cláusula `GROUP BY` agrupa as linhas de um `result set` pelo valor da coluna informada como parâmetro. Quando as linhas são agrupadas, podemos executar funções de agregação (MAX, MIN, AVG, SUM e COUNT) com os valores das colunas.

## Exemplos

- função COUNT() - Total de faixas por álbum

```sql
SELECT
	Title AS Album,
	COUNT(trackid) AS 'Número de Faixas'
FROM albums
INNER JOIN
	tracks USING (AlbumId)
GROUP BY AlbumId
LIMIT 10;
```

- função SUM() - Total de quanto cada cliente já comprou.

```sql
SELECT
	CustomerId, FirstName, SUM(Total)
FROM
	invoices
INNER JOIN
	customers USING (CustomerId)
GROUP BY
	CustomerId ORDER BY CustomerId;
```

- função AVG() - Média de quanto já foi vendido para cada país.

```sql
SELECT
	BillingCountry, AVG(Total)
FROM
	invoices
GROUP BY
	BillingCountry
ORDER BY 
	AVG(Total) DESC;
```

- função MAX() - Quais clientes, de cada país, foram os que fizeram o pedido de maior valor e qual o valor desse pedido.

```sql
SELECT 
	BillingCountry, CustomerId, MAX(Total), FirstName
FROM
	invoices
INNER JOIN customers USING(CustomerId)
	GROUP BY BillingCountry
ORDER BY MAX(Total) DESC;
```

- Relatório por cliente:
	- De qual país é o cliente
	- Quanto comprou no total - SUM()
	- Valor da maior compra - MAX()
	- Valor da menor compra - MIN()
	- Média de valor das compras - AVG()

```sql
SELECT
	CustomerId,
	FirstName || ' ' || LastName AS Cliente,
	BillingCountry,
	SUM(Total),
	MAX(Total),
	MIN(Total),
	AVG(Total)
FROM invoices
INNER JOIN
	customers USING(CustomerId)
GROUP BY CustomerId
ORDER BY FirstName, LastName;
```

