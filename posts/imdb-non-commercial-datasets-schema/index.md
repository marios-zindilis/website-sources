---
title: IMDB non-commercial datasets schema
description: Relationship diagram for the datasets provided by IMDB for personal and non-commercial use
first-published: 2023-11-04
tags:
- IMDb
---

[IMDb][1] provides a subset of their data in tab-separated format for personal and non-commercial use. You can find more
information, including legal, at [IMDb Non-Commercial Datasets][2]. These are some notes on the schema.

<!-- Links -->
[1]: https://imdb.com/ "IMDB"
[2]: https://developer.imdb.com/non-commercial-datasets/ "IMDb Non-Commercial Datasets"

<!-- read more -->

There are 7 files provided, at the time of writing this article:

| # | Name                      | Compressed size (MB) | Uncompressed size (MB) | Number of rows
| - |-------------------------- | -------------------: | ---------------------: | ------------:
| 1 | `name.basics.tsv.gz`      |                  245 |                    753 |     12,981,035
| 2 | `title.akas.tsv.gz`       |                  305 |                   1783 |     37,728,267
| 3 | `title.basics.tsv.gz`     |                  172 |                    841 |     10,285,368
| 4 | `title.crew.tsv.gz`       |                   66 |                    325 |     10,285,368
| 5 | `title.episode.tsv.gz`    |                   41 |                    196 |      7,844,603
| 6 | `title.principals.tsv.gz` |                  436 |                   2475 |     58,914,239
| 7 | `title.ratings.tsv.gz`    |                    7 |                     23 |      1,366,240

There are 2 unique alphanumeric identifiers in those files:

1.  `tconst` is an ID for a title, and
2.  `nconst` is an ID for a name.

This diagram shows the relationships between the 7 data exports. This isn't exactly an entity relationship diagram, but
it's not too far either.

![](/static/img/imdb-non-commercial-datasets-diagram.png)

This diagram was created using [DBML](https://dbml.dbdiagram.io/docs/) and can be imported into
[dbdiagram.io](https://dbdiagram.io/). Here's the code:
 

```
Table name_basics {
  nconst string [primary key]
  primaryName string
  birthYear number
  deathYear number
  primaryProfession string_array
  knownForTitles nconst_array [ref: < title_basics.tconst]
}

Table title_basics {
  tconst string [primary key]
  titleType string
  primaryTitle string
  originalTitle string
  isAdult boolean
  startYear number
  endYear number
  runtimeMinutes number
  genres string_array
}

Table title_akas {
  titleId string [ref: > title_basics.tconst]
  ordering integer
  title string
  region string
  language string
  types string_array
  attributes string_array
  isOriginalTitle boolean
}

Table title_crew {
  tconst string [ref: - title_basics.tconst]
  directors nconst_array [ref: > name_basics.nconst]
  writers nconst_array [ref: > name_basics.nconst]
}

Table title_episode {
  tconst string [primary key]
  parentTconst string [ref: > title_basics.tconst]
  seasonNumber number
  episodeNumber number
}

Table title_principals {
  tconst string [ref: - title_basics.tconst]
  ordering number
  nconst string [ref: - name_basics.nconst]
  category string
  job string
  characters string
}

Table title_ratings {
  tconst string [ref: - title_basics.tconst]
  averageRating number
  numVotes number
}
```

## 5 First Rows ##

Here's a sample of the data, these are the first 5 rows from each export.

### name.basics.tsv ###

| nconst | primaryName | birthYear | deathYear | primaryProfession | knownForTitles
|--------|-------------|----------:|----------:|-------------------|---------------
nm0000001 | Fred Astaire | 1899 | 1987 | soundtrack,actor,miscellaneous | tt0050419,tt0053137,tt0072308,tt0031983
nm0000002 | Lauren Bacall | 1924 | 2014 | actress,soundtrack | tt0075213,tt0037382,tt0038355,tt0117057
nm0000003 | Brigitte Bardot | 1934 | \N | actress,soundtrack,music_department | tt0054452,tt0056404,tt0057345,tt0049189
nm0000004 | John Belushi | 1949 | 1982 | actor,soundtrack,writer | tt0080455,tt0072562,tt0077975,tt0078723
nm0000005 | Ingmar Bergman | 1918 | 2007 | writer,director,actor | tt0083922,tt0069467,tt0050986,tt0050976

### title.akas.tsv ###

| titleId | ordering | title | region | language | types | attributes | isOriginalTitle
|---------|----------|-------|--------|----------|-------|------------|----------------
| tt0000001 | 1 | Карменсіта | UA | \N | imdbDisplay | \N | 0
|tt0000001 | 2 | Carmencita | DE | \N | \N | literal title | 0
|tt0000001 | 3 | Carmencita - spanyol tánc | HU | \N | imdbDisplay | \N | 0
|tt0000001 | 4 | Καρμενσίτα | GR | \N | imdbDisplay | \N | 0
|tt0000001 | 5 | Карменсита | RU | \N | imdbDisplay | \N | 0


### title.basics.tsv ###
|tconst | titleType | primaryTitle | originalTitle | isAdult | startYear | endYear | runtimeMinutes | genres
|-|-|-|-|-|-:|-|-:|-
tt0000001 | short | Carmencita | Carmencita | 0 | 1894 | \N | 1 | Documentary,Short
tt0000002 | short | Le clown et ses chiens | Le clown et ses chiens | 0 | 1892 | \N | 5 | Animation,Short
tt0000003 | short | Pauvre Pierrot | Pauvre Pierrot | 0 | 1892 | \N | 4 | Animation,Comedy,Romance
tt0000004 | short | Un bon bock | Un bon bock | 0 | 1892 | \N | 12 | Animation,Short
tt0000005 | short | Blacksmith Scene | Blacksmith Scene | 0 | 1893 | \N | 1 | Comedy,Short

### title.crew.tsv ###
tconst | directors | writers
-|-|-
tt0000001 | nm0005690 | \N
tt0000002 | nm0721526 | \N
tt0000003 | nm0721526 | \N
tt0000004 | nm0721526 | \N
tt0000005 | nm0005690 | \N

### title.episode.tsv ###
tconst | parentTconst | seasonNumber | episodeNumber
-|-|-:|-:
tt0041951 | tt0041038 | 1 | 9
tt0042816 | tt0989125 | 1 | 17
tt0042889 | tt0989125 | \N | \N
tt0043426 | tt0040051 | 3 | 42
tt0043631 | tt0989125 | 2 | 16

### title.principals.tsv ###
tconst | ordering | nconst | category | job | characters
-------|---------:|--------|----------|-----|-----------
tt0000001 | 1 | nm1588970 | self | \N | ["Self"]
tt0000001 | 2 | nm0005690 | director | \N | \N
tt0000001 | 3 | nm0374658 | cinematographer | director of photography | \N
tt0000002 | 1 | nm0721526 | director | \N | \N
tt0000002 | 2 | nm1335271 | composer | \N | \N

### title.ratings.tsv ###
tconst | averageRating | numVotes
-|-:|-:
tt0000001 | 5.7 | 2004
tt0000002 | 5.8 | 269
tt0000003 | 6.5 | 1903
tt0000004 | 5.5 | 178
tt0000005 | 6.2 | 2685
