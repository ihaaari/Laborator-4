# Laborator-4(some source code)

final AtomicLong counter = new AtomicLong(0);

final Function<IonReader, Integer> callback = (reader) -> {
    counter.addAndGet(reader.intValue());

    return 0;
};

final PathExtractor<?> pathExtractor = PathExtractorBuilder.standard()
    .withSearchPath("(foo)", callback)
    .withSearchPath("(bar)", callback)
    .withSearchPath("(A::baz 1)", callback)
    .build();

final IonReader ionReader = IonReaderBuilder.standard().build("{foo: 1}"
    + "{bar: 2}"
    + "{baz: A::[10,20,30,40]}"
    + "{baz: [100,200,300,400]}"
    + "{other: 99}"
);

pathExtractor.match(ionReader);

assertEquals(23, counter.get());
