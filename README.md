================
**_basic commands_**
================

**1. Find all documents by field in array field with projection**

        Bson queryFilter = new Document("countries", new Document("$all", Arrays.asList(country))) // country - array;
        Bson projection = new Document("title", 1);
        List<Document> movies = new ArrayList<>();
        moviesCollection
            .find(queryFilter)
            .projection(projection)
            .into(movies);

        return movies;

**2. Find all documents by field in array field with sort**

        sortKey = "tomatoes.viewer.numReviews";Bson sort = Sorts.descending(sortKey);
        Bson castFilter = new Document("cast", new Document("$all", Arrays.asList(cast))) // cast - array;

        List<Document> movies = new ArrayList<>();
        moviesCollection
                .find(castFilter)
                .sort(sort)
                .limit(limit)
                .skip(skip)
                .iterator()
                .forEachRemaining(movies::add);
        return movies;
