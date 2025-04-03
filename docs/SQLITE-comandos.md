# Comandos do SQLite

Para rodar o sqlite

```
>sqlite3
SQLite version 3.43.1 2023-09-11 12:01:27
Enter ".help" for usage hints.
Connected to a transient in-memory database.
Use ".open FILENAME" to reopen on a persistent database.
sqlite>
```

Por padrão o SQLite inicia abrindo um banco de dados em memória, não um arquivo de banco de dados.

Para abrir um arquivo de banco de dados usamos o comando `.open ARQUIVO`

```
sqlite> .open chinook.db
```

O arquivo também pode ser aberto em linha de comando.

```
>sqlite3 chinook.db
SQLite version 3.43.1 2023-09-11 12:01:27
Enter ".help" for usage hints.
sqlite>
```

## Criar um banco de dados

Para criar um arquivo de banco de dados basta iniciar uma sessão do SQLite com um nome de arquiv que não existe. O SQLite vai criar o arquivo.

Para criar um arquivo chamado `agenda.db`, digite:

```
>sqlite3 agenda.db
SQLite version 3.43.1 2023-09-11 12:01:27
Enter ".help" for usage hints.
sqlite>
```

## Ajuda

O comando `.help` lista todos os comandos do SQLite e também dá informações específicas sobre um comando em particular.

## Listar bancos de dados da sessão de uso atual

O comando `.databases` lista todos os bancos de dados abertos

```
sqlite> .databases
seq  name             file
---  ---------------  --------------------------
0    main             c:\sqlite\db\sales.db
sqlite>
```

## Listar as tabelas de um banco de dados

Para listar todas as tabelas do banco de dados atual, use `.tables`

```
sqlite> .tables
albums          employees       invoices        playlists
artists         genres          media_types     tracks
customers       invoice_items   playlist_track
sqlite>
```

## Mostar a estrutura de uma tabela

O SQLite tem uma função que imprime a estrutura de uma tabela, chamada `table_info()`. Executamos essa função com um comando `PRAGMA`, assim:

```
sqlite>PRAGMA table_info(albums);
```

O resultado é

```
sqlite> PRAGMA table_info(albums);
0|AlbumId|INTEGER|1||1
1|Title|NVARCHAR(160)|1||0
2|ArtistId|INTEGER|1||0
sqlite>
```

O modo como as informações são exibidas não é dos melhores, mas podemos mudar.

Digite:

```
sqlite>.mode table
```

Agora repita o comando `PRAGMA table_info(albums);` e resultado agora deve ser:

```
sqlite> PRAGMA table_info(albums);
+-----+----------+---------------+---------+------------+----+
| cid |   name   |     type      | notnull | dflt_value | pk |
+-----+----------+---------------+---------+------------+----+
| 0   | AlbumId  | INTEGER       | 1       |            | 1  |
| 1   | Title    | NVARCHAR(160) | 1       |            | 0  |
| 2   | ArtistId | INTEGER       | 1       |            | 0  |
+-----+----------+---------------+---------+------------+----+
sqlite>
```

## Salvar o resultado de uma consulta em um arquivo

O comando `.output ARQUIVO` redireciona o resultado de todas as consultas para um arquivo. 

> Se você quer salvar apenas o resultado de 1 consulta, use `.once ARQUIVO`. Assim os próximos resultados voltam a ser mostrados na tela

``` 
sqlite> .output albums.txt
sqlite> SELECT title FROM albums;
```

## Executar SQL a partir de um arquivo

Se tivermos um arquivo chamado `consulta.txt` com a seguinte consulta

```
SELECT albumid, title
FROM albums
ORDER BY title
LIMIT 10;
```

Para rodar essa consulta usamos o comando `.read ARQUIVO`.

```
sqlite> .mode column
sqlite> .header on
sqlite> .read consulta.txt
AlbumId     Title
----------  ----------------------
156         ...And Justice For All
257         20th Century Masters -
296         A Copland Celebration,
94          A Matter of Life and D
95          A Real Dead One
96          A Real Live One
285         A Soprano Inspired
139         A TempestadeTempestade
203         A-Sides
160         Ace Of Spades
```

## Sair

Para sair do SQLite digite `.exit`

```
sqlite>.exit
```
