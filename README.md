================
**_basic commands_**
================

**1. Find all documents by field in array field with projection**

`Bson queryFilter = new Document("countries", new Document("$all", Arrays.asList(country)));

Bson projection = new Document("title", 1);

List<Document> movies = new ArrayList<>();

moviesCollection
    .find(queryFilter)
    .projection(projection)
    .into(movies);`