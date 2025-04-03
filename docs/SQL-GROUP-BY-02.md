# Mais exemplos com GROUP BY

- Clientes que gastaram mais que 3x acima da média, separados por país.

Observar que é preciso agrupar também por cliente para que que cada cliente entre no result set, senão o agrupamento por país prevalece e apenas 1 cliente é exibido mesmo havendo mais de 1 para o país.

```sql
SELECT
	BillingCountry,
	CustomerId,
	SUM(Total)
FROM
	invoices
WHERE
	Total > (SELECT AVG(Total)*3 FROM invoices)
GROUP BY
	BillingCountry, CustomerId;
```

- 

```sql
SELECT
	BillingCountry,
	CustomerId,
	SUM(Total),
	CASE 
		WHEN SUM(Total)>40 THEN '*'
		ELSE ''
	END
FROM invoices
GROUP BY
	BillingCountry, CustomerId;
```

-

```sql
SELECT
	BillingCountry,
	CustomerId,
	SUM(Total),
	CASE
		WHEN SUM(Total)>40 THEN
			'*'
		ELSE ''
		END AS Alto
FROM
	invoices
GROUP BY
	BillingCountry, CustomerId;
HAVING
	Alto = '*';
```

- 

```sql
SELECT
	tracks.Name AS Faixa,
	albums.Title AS Álbum,
	genres.Name AS Gênero,
	(SELECT Milliseconds/60000) | 'min' AS min
FROM
	tracks
INNER JOIN
	albums USING(AlbumId)
INNER JOIN
	genres USING(GenreId)
WHERE
	genres.Name NOT IN (
		'TV Shows',
		'Drama',
		'Sci Fi & Fantasy',
		'Science Fiction',
		'Comedy'
		)
GROUP BY AlbumId, TrackId
HAVING min>10;
```

-

```sql
SELECT
	tracks.Name AS Faixa,
	albums.Title AS Álbum,
	genres.Name AS Gênero,
	(SELECT Milliseconds/60000)||'min' AS Tempo
FROM
	tracks
INNER JOIN
	albums USING(AlbumId)
INNER JOIN
	genres USING(GenreId)
WHERE
	genres.Name NOT IN (
		'TV Shows',
		'Drama',
		'Sci Fi & Fantasy',
		'Science Fiction',
		'Comedy'
	)
GROUP BY
	AlbumId, TrackId
HAVING (SELECT Milliseconds/60000) > 10;
```

