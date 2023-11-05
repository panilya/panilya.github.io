+++ 
author = "Illia Pantsyr" 
title = "Datafaker 2.0" 
date = "2023-06-14" 
tags = [ "java", "jvm", "datafaker", "open-source" ] 
+++

One of the major changes in Datafaker 2.0 is the requirement for Java version 17 as a minimum, similar to popular frameworks such as Spring Boot 3.0. This release of Datafaker brings significant improvements to the library's performance, support of Java Records, capability to generate larger amounts of test data, and much more.

## Schemas and transformers

The most common use case for Datafaker is to generate random readable values in Java, such as firstnames, quotes or other values. 

But what if you need to generate data in some other format, such as JSON or CSV?
For this purpose, Datafaker has a Schema and Transformer concept which work hand in hand and should therefore be used together.

Let's take a look at what this Schema and Transformer are and what role they play.

A Schema is a set of rules that describe what needs to be done to convert data from a Datafaker format to one of the supported formats. One of the main advantages of a schema is that the same schema can be used to convert to different formats.

In Datafaker, each format has its own Transformer implementation. At the time of writing, Datafaker supports 6 formats, which means there are also 6 Transformer implementations, namely:
 ⁃ CSV (CsvTransformer)
 ⁃ JSON (JsonTrasformer)
 ⁃ SQL (SqlTransformer)
 ⁃ YAML (YamlTransformer)
 ⁃ XML (XmlTransformer)
 ⁃ Java Object (JavaObjectTransformer)

Let's take a closer look at one of the formats, in this case, the CSV format.

### Generating CSV files

When working with CSV files, there may be times when you need to create CSV files with data, for example for testing purposes. Manually entering data into a CSV file can be a tedious and time-consuming process. Instead of wasting time and effort on manual data entry, you can use a tool like Datafaker to quickly generate as much fake data as you need in a CSV format. Datafaker allows you to specify the number of rows and columns you need, and generate randomized data that can be easily exported to a CSV file. This can be especially useful when working with large datasets or when testing and prototyping your applications.

Let's look at an example:

``` java
        Faker faker = new Faker(Locale.GERMANY); // Datafaker supports any locale from the Locale package
        Schema<Object, ?> schema = Schema.of( // Define a schema, which we can use in any Transformer
            field("firstName", () -> faker.name().firstName()),
            field("lastName", () -> faker.name().lastName()),
            field("phoneNumber", () -> faker.phoneNumber().phoneNumberInternational()));
        
        CsvTransformer<Object> csvTransformer = CsvTransformer.builder().build(); // Instantiate CsvTransformer using the appropriate builder.
        System.out.println(csvTransformer.generate(schema, 5)); // This is where the magic happens. Call the `generate` method on the transformer with your schema plus the number of records to get the result as a string.
```

A possible result that can be obtained by executing the code above can be found below:

``` csv
"firstName";"lastName";"phoneNumber"
"Jannik";"Ripken";"+49 9924 780126"
"Frederike";"Birkemeyer";"+49 2933 945975"
"Michelle";"Steuk";"+49 7033 814683"
"Ellen";"Semisch";"+49 9537 991422"
"Oskar";"Habel";"+49 3626 169891"
```

The CsvTransformer builder also gives you several parameters to use, such as:

- `quote()`, in case you want to change the default(`"`) quote.
- `separator()`, the character which delimits columns in rows.
- `header()` boolean parameter which toggles the generation of the header in the resulted CSV.

All other 5 formats work on the same principle, except for the Java Object format.

Let's see what is so special about this Java Object transformation.

### JavaObjectTransformer

JavaObjectTransformer is available since version 1.8.0 of Datafaker. This was further enhanced in Datafaker 2.0 by providing support for Java Records in the JavaObjectTransformer.

In total, we have two ways to work with the JavaObjectTransformer:

1) Use JavaObjectTransformer and Schema.
2) Use a predefined Schema.

#### JavaObjectTransformer with Transformer and Schema pair

Let's take a closer look at each option. The first way to use it is through the familiar Transformer and Schema pair. Let's look at a practical example:

Note that we use Java 17 and Datafaker 2.0. Therefore, we will create a `Client` Java Record with 3 properties:

``` java
public record Client(String firstName, String lastName, String phoneNumber) { }
```

Next, we need to provide the Schema for our class.

``` java
        Faker faker = new Faker();
        JavaObjectTransformer jTransformer = new JavaObjectTransformer();
        
        Schema<Object, ?> schema = Schema.of(
                field("firstName", () -> faker.name().firstName()),
                field("lastName", () -> faker.name().lastName()),
                field("phoneNumber", () -> faker.phoneNumber().phoneNumberInternational())
        );
        System.out.println(jTransformer.apply(Client.class, schema));
```

The result of our program will be as follows: 
`Client{firstName='Elton', lastName='Conroy', phoneNumber='+1 808-239-0480'}`

#### JavaObjectTransformer with Transformer and predefined Schema

Let's take a look at the second use case, namely, populating Java objects with a predefined schema.
The essence of this approach is to override the schema and use it without explicitly specifying it every time.

Let's move on to a practical example:

First of all, we need to define our schema for our model. It is defined as follows and, on my machine, located in the `com.datafaker` package in the `DatafakerSchema` class:

``` java
    public static Schema<Object, ?> defaultSchema() {
        var faker = new Faker(Locale.UK, new RandomService(new Random(1)));
        return Schema.of(
                field("firstName", () -> faker.name().firstName()),
                field("lastName", () -> faker.name().lastName()),
                field("phoneNumber", () -> faker.phoneNumber().phoneNumberInternational())
        );
    }
```

Then you should provide a class to be used as a template for generated objects. This class should be annotated with the `@FakeForSchema` annotation with the path to the schema method as a value.
> Note: If the default schema and the class template are in the same class, you can omit the full path to the method and use only the method name.

``` java
@FakeForSchema("com.datafaker.DatafakerSchema#defaultSchema")
public record Client(String firstName, String lastName, String phoneNumber) { }
```

Then you can use `net.datafaker.providers.base.BaseFaker.populate(java.lang.Class<T>)` to populate the object with the default predefined schema.

``` java
    Client client = BaseFaker.populate(Client.class);
```

Alternatively, you can use `net.datafaker.providers.base.BaseFaker.populate(java.lang.Class<T>, net.datafaker.schema.Schema<java.lang.Object, ?>)` to populate the object with a custom schema:

``` java
    Client client = BaseFaker.populate(Client.class, Schema.of(
                field("firstName", () -> faker.name().firstName()),
                field("lastName", () -> faker.name().lastName()),
                field("phoneNumber", () -> faker.phoneNumber().phoneNumberInternational())
        ));
```

The result of both `populate` methods will be the same:
`Client{firstName='Darrel', lastName='Bogisich', phoneNumber='+44 161-444-6889'}`

If you want to know more, you can find more information about Schema and Transformers in the official documentation of [Datafaker](https://www.datafaker.net/documentation/schemas/).

## Sequences

Sequences are a very powerful but undervalued feature of Datafaker. Imagine a situation where you need just a `java.util.List` of random values or you need to have an infinite stream of random data. In such situations, Datafaker sequences are a great fit. But before you start using it, it's good to know the details of sequences and their types, and that's what we're going to do.

Datafaker supports two types of fake sequences:

- FakeCollection
- FakeStream

FakeCollection is used to generate an in-memory list of fake values. Let's take a look at a practical example:

``` java
        Faker faker = new Faker();
        List<String> names =
                faker.collection(
                                () -> faker.name().fullName(),
                                () -> faker.address().city())
                        .len(2, 6)
                        .generate();
                        
        System.out.println(names);
```

This code example will generate a List of fake values, where each element will either be a full name or a city, with a length between 2 and 6 elements. This is a possible result of the code above:

`[Pete Nienow, Erik Kub, New Jerrie, Tempie Erdman]`

FakeStream is used to generate a `java.util.stream.Stream` of fake values. FakeStream is similar to FakeCollection, however, there are some important differences. For example, FakeStream can be infinite and the result of the FakeStream is a Stream object with generated values instead of a collection.

Imagine you need to model a constant stream of data. For example, the temperature from an IoT temperature sensor. In this case, FakeStream will be the right choice. Let's see how to write this in code:

``` java
        Faker faker = new Faker();
        Stream<Object> objects =
                faker.<Object>stream(() -> faker.random().nextDouble(20, 22))
                        .generate();
        objects.forEach(System.out::println);
```

This code will generate a constant stream of doubles between 20 and 22. However, it's also possible to specify the length of the stream via the `len(int minLength, int maxLength)` or `len(int length)` parameter.

``` java
        Faker faker = new Faker();
        Stream<Object> objects =
                faker.<Object>stream(() -> faker.random().nextDouble(20, 22))
                        .len(3, 5)
                        .generate();
        objects.forEach(System.out::println);
```

The possible result may be: `21.663129306577716
20.228104917791825
20.559417738876828`

## Conclusion

In conclusion, Datafaker 2.0 is a significant upgrade to an already impressive data generation library. With its diverse data generation options and improved scalability, Datafaker 2.0 is an essential tool for developers looking to generate realistic and diverse data for testing and development purposes. For more information, head over to [Datafaker documentation.](https://www.datafaker.net/documentation/getting-started/) and don't forget to star [Datafaker](https://github.com/datafaker-net/datafaker) on the GitHub!
