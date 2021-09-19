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

**3. Faceted Search**

        //String... cast
        List<Document> movies = new ArrayList<>();
        
        Bson skipStage = Aggregates.skip(skip);
        Bson matchStage = Aggregates.match(Filters.in("cast", cast));
        String sortKey = "tomatoes.viewer.numReviews";
        Bson sortStage = Aggregates.sort(Sorts.descending(sortKey));
        Bson limitStage = Aggregates.limit(limit);
        Bson facetStage = buildFacetStage();

        List<Bson> pipeline = new LinkedList<>();
        pipeline.add(matchStage);
        pipeline.add(sortStage);
        pipeline.add(skipStage);
        pipeline.add(limitStage);
        pipeline.add(facetStage);

        moviesCollection.aggregate(pipeline).iterator().forEachRemaining(movies::add);
        return movies;
