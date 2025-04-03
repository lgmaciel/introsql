# JOIN

Vamos estudar uma situação prática de uso de um banco de dados relacional, como o SQLite, e ver como trabalhar com a cláusula `JOIN` para resolvê-la.

A situação é o caso em que precisamos consultar mais de uma tabela para ter os dados que precisamos para uma consulta. Por exemplo, considerando a tabela `tracks`:

```
+-----+--------------+---------------+---------+------------+----+
| cid |     name     |     type      | notnull | dflt_value | pk |
+-----+--------------+---------------+---------+------------+----+
| 0   | TrackId      | INTEGER       | 1       |            | 1  |
| 1   | Name         | NVARCHAR(200) | 1       |            | 0  |
| 2   | AlbumId      | INTEGER       | 0       |            | 0  |
| 3   | MediaTypeId  | INTEGER       | 1       |            | 0  |
| 4   | GenreId      | INTEGER       | 0       |            | 0  |
| 5   | Composer     | NVARCHAR(220) | 0       |            | 0  |
| 6   | Milliseconds | INTEGER       | 1       |            | 0  |
| 7   | Bytes        | INTEGER       | 0       |            | 0  |
| 8   | UnitPrice    | NUMERIC(10,2) | 1       |            | 0  |
+-----+--------------+---------------+---------+------------+----+

```

Todas as faixas cadastradas nessa tabela pertencem a algum álbum e cada álbum tem um número identificador único, ou seja, um número que foi designado a esse álbum e a nenhum outro mais. Esse é o valor que está na coluna `AlbumId`. 

Então, se quisermos saber quais os nomes das faixas que pertencem ao álbum cadastrado com id=1 escrevemos:

```sql
SELECT
	name
FROM
	tracks
WHERE
	albumid = 1;
```

e a resposta deve ser:

```
+-----------------------------------------+
|                  Name                   |
+-----------------------------------------+
| For Those About To Rock (We Salute You) |
| Put The Finger On You                   |
| Let's Get It Up                         |
| Inject The Venom                        |
| Snowballed                              |
| Evil Walks                              |
| C.O.D.                                  |
| Breaking The Rules                      |
| Night Of The Long Knives                |
| Spellbound                              |
+-----------------------------------------+
```

Sabemos agora que todas essas faixas pertencem ao mesmo álbum mas não temos informações sobre o álbum, além de seu número de identificação.

Vamos ler essas informações sobre o álbum fazendo uma segunda consulta, desta vez à tabela `albums`.

```sql
SELECT
	title
FROM
	albums
WHERE
	albumid=1;
```

E a resposta é:

```
+---------------------------------------+
|                 Title                 |
+---------------------------------------+
| For Those About To Rock We Salute You |
+---------------------------------------+
```

Ótimo! Porém ainda não sabemos de quem é o álbum. 

Consultando a estrutura da tabela `albums` descobrimos que lá a informação sobre de quem é o álbum está amazenada na coluna `ArtistId`. Esse é o identificador de um `artista`. 

```  
sqlite> PRAGMA table_info(albums);
+-----+----------+---------------+---------+------------+----+
| cid |   name   |     type      | notnull | dflt_value | pk |
+-----+----------+---------------+---------+------------+----+
| 0   | AlbumId  | INTEGER       | 1       |            | 1  |
| 1   | Title    | NVARCHAR(160) | 1       |            | 0  |
| 2   | ArtistId | INTEGER       | 1       |            | 0  |
+-----+----------+---------------+---------+------------+----+
```

Os demais dados de um artista, entretanto, não estão nessa tabela. Eles estão na tabela `artists`.

Então, para saber qual o artista desse álbum temos que consultar o valor de seu `ArtistId` na tabela `albums` *e também* o registro completo na tabela `artists` que tem `ArtistId` igual a esse valor.

Consultando `ArtistId` em `albums`

```sql
SELECT
	Title, ArtistId
FROM
	albums
WHERE
	albumid = 1;
```

A resposta deve ser:
```
+---------------------------------------+----------+
|                 Title                 | ArtistId |
+---------------------------------------+----------+
| For Those About To Rock We Salute You | 1        |
+---------------------------------------+----------+
```

Então, coincidentemente o `ArtistId` desse álbum é 1 também. Agora podemos procurar qual artista corresponde a esse `ArtistId` na tabela `artists`.

```sql
SELECT * FROM artists WHERE ArtistId = 1;
```

E descobrimos que o nome do artista é `AC/DC`.

```
+----------+-------+
| ArtistId | Name  |
+----------+-------+
| 1        | AC/DC |
+----------+-------+
```


Agora temos as informações completas sobre cada faixa, mas elas ficaram dispersas em três `result sets` diferentes e o ideal seria ter todas elas juntas em um mesmo `result set`. Assim:

```
+-----------------------------------------+---------------------------------------+---------+
|                  Faixa                  |                Título                 | Artista |
+-----------------------------------------+---------------------------------------+---------+
| For Those About To Rock (We Salute You) | For Those About To Rock We Salute You | AC/DC   |
| Put The Finger On You                   | For Those About To Rock We Salute You | AC/DC   |
| Let's Get It Up                         | For Those About To Rock We Salute You | AC/DC   |
| Inject The Venom                        | For Those About To Rock We Salute You | AC/DC   |
| Snowballed                              | For Those About To Rock We Salute You | AC/DC   |
| Evil Walks                              | For Those About To Rock We Salute You | AC/DC   |
| C.O.D.                                  | For Those About To Rock We Salute You | AC/DC   |
| Breaking The Rules                      | For Those About To Rock We Salute You | AC/DC   |
| Night Of The Long Knives                | For Those About To Rock We Salute You | AC/DC   |
| Spellbound                              | For Those About To Rock We Salute You | AC/DC   |
+-----------------------------------------+---------------------------------------+---------+
```

Note ainda que o nome das colunas foi personalizado. 


# INNER JOIN

Usamos a cláusula `JOIN` para *juntar* em uma mesma consulta colunas que estão em tabelas diferentes.

Para escrever uma consulta que retorne um `result set` que contenha a coluna `Name` da tabela `tracks` e também a coluna `Title` da tabela `albums` escrevemos, por exemplo:

```sql
SELECT
	Name, Title
FROM
	tracks AS a
INNER JOIN
	albums AS b
	ON a.AlbumId = b.AlbumId
LIMIT 15;
```

Nesse exemplo usamos `AS` para chamar a tabela `tracks` de tabela A e albums de tabela B. Nós declaramos duas coisas para escrever a cláusula `INNER JOIN`:

1. a tabela de onde virão as colunas extras para a consulta - tabela B
2. o critério usado para juntar os dados de um registro da tabela B com um registro da tabela A, formando uma linha de registro única no `result set` com dados vindos das duas tabelas.

Nesse caso, o SQL vai juntar os dados quando o valor de `AlbumId` do registro da tabela B for o mesmo que o valor de AlbumId da tabela A. Ou seja, sempre que um registro da tabela B contiver dados do mesmo Album a que se refere o registro na tabela A.

```
+-----------------------------------------+---------------------------------------+
|                  Name                   |                 Title                 |
+-----------------------------------------+---------------------------------------+
| For Those About To Rock (We Salute You) | For Those About To Rock We Salute You |
| Put The Finger On You                   | For Those About To Rock We Salute You |
| Let's Get It Up                         | For Those About To Rock We Salute You |
| Inject The Venom                        | For Those About To Rock We Salute You |
| Snowballed                              | For Those About To Rock We Salute You |
| Evil Walks                              | For Those About To Rock We Salute You |
| C.O.D.                                  | For Those About To Rock We Salute You |
| Breaking The Rules                      | For Those About To Rock We Salute You |
| Night Of The Long Knives                | For Those About To Rock We Salute You |
| Spellbound                              | For Those About To Rock We Salute You |
| Balls to the Wall                       | Balls to the Wall                     |
| Fast As a Shark                         | Restless and Wild                     |
| Restless and Wild                       | Restless and Wild                     |
| Princess of the Dawn                    | Restless and Wild                     |
| Go Down                                 | Let There Be Rock                     |
+-----------------------------------------+---------------------------------------+
```


Para que o `result set` contenha as faixas de um só álbum, usamos a cláusula `WHERE` e agora retiramos o `LIMIT` que não faz mais sentido neste caso.

```sql
SELECT
	Name, Title
FROM
	tracks AS a
INNER JOIN
	albums AS b
	ON a.AlbumId = b.AlbumId
WHERE a.AlbumId = 1;
```


E temos como resultado:

```
+-----------------------------------------+---------------------------------------+
|                  Name                   |                 Title                 |
+-----------------------------------------+---------------------------------------+
| For Those About To Rock (We Salute You) | For Those About To Rock We Salute You |
| Put The Finger On You                   | For Those About To Rock We Salute You |
| Let's Get It Up                         | For Those About To Rock We Salute You |
| Inject The Venom                        | For Those About To Rock We Salute You |
| Snowballed                              | For Those About To Rock We Salute You |
| Evil Walks                              | For Those About To Rock We Salute You |
| C.O.D.                                  | For Those About To Rock We Salute You |
| Breaking The Rules                      | For Those About To Rock We Salute You |
| Night Of The Long Knives                | For Those About To Rock We Salute You |
| Spellbound                              | For Those About To Rock We Salute You |
+-----------------------------------------+---------------------------------------+

```

## USING

É possível simplificar a condição da cláusula `ON` nos casos em que as colunas das tabelas tem o mesmo nome. E esse é o caso na consulta que fizemos. As tabelas A e B tem como ponto em comum a coluna `AlbumId`.

Para isso, usamos a condição `USING (nome_da_coluna_em_comum)`. Assim:

```sql
SELECT
	Name, Title
FROM
	tracks AS a
INNER JOIN
	albums AS b
	USING(AlbumId)
WHERE a.AlbumId = 1;
```

O `result set` desa consulta é o mesmo que o da anterior. 


## JOIN com várias tabelas

Para fazer uma consulta em mais de duas tabelas ao mesmo tempo, basta acrescentar mais cláusual `JOIN` ao `SELECT`.

Por exemplo, para incluir também o nome do artista do álbum onde está cada faixa, precisamos consultar 3 tabela: `tracks`, `albums` e `artists`.

> Vamos dar apelidos para cada uma das tabelas para não introduzir uma ambiguidade na consulta, já que tanto na tabela `tracks` como na tabela `artists` há uma coluna chamada `Name`.

A consulta fica assim:

```
SELECT
	tr.Name, al.Title, ar.Name
FROM
	tracks AS tr
INNER JOIN 
	albums AS al USING(AlbumId)
INNER JOIN
	artists AS ar USING(ArtistId)
WHERE
	tr.AlbumId = 1;
```


O resultado é

```
+-----------------------------------------+---------------------------------------+-------+
|                  Name                   |                 Title                 | Name  |
+-----------------------------------------+---------------------------------------+-------+
| For Those About To Rock (We Salute You) | For Those About To Rock We Salute You | AC/DC |
| Put The Finger On You                   | For Those About To Rock We Salute You | AC/DC |
| Let's Get It Up                         | For Those About To Rock We Salute You | AC/DC |
| Inject The Venom                        | For Those About To Rock We Salute You | AC/DC |
| Snowballed                              | For Those About To Rock We Salute You | AC/DC |
| Evil Walks                              | For Those About To Rock We Salute You | AC/DC |
| C.O.D.                                  | For Those About To Rock We Salute You | AC/DC |
| Breaking The Rules                      | For Those About To Rock We Salute You | AC/DC |
| Night Of The Long Knives                | For Those About To Rock We Salute You | AC/DC |
| Spellbound                              | For Those About To Rock We Salute You | AC/DC |
+-----------------------------------------+---------------------------------------+-------+
```

