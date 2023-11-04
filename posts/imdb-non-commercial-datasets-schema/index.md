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

1.  `name.basics.tsv.gz`
2.  `title.akas.tsv.gz`
3.  `title.basics.tsv.gz`
4.  `title.crew.tsv.gz`
5.  `title.episode.tsv.gz`
6.  `title.principals.tsv.gz`
7.  `title.ratings.tsv.gz`

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
