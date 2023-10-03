Introduction
============

This project contains logic to receive an HTTP data stream and store it the file system. The aim is to test Camel's ability to handle large HTTP data streams.


Running a positive test
=======================

To demonstrate Camel can handle larger data streams than memory allocated to the JVM, we need to start it with low memory settings.

To run it follow the commads below:

1. Package the project in a JAR file:

	./mvnw clean package


2. Run the JAR file with low memory settings:

	java -jar -Xms200m -Xmx200m target/demo-1.0.0.jar


3. From the client folder send a request using a large data stream.


4. Stop Camel Spring Boot and inspect the result file.

  Camel Spring Boot should process the HTTP byte stream and dump it in a file in the 'data' directory.


Running a negative test
=======================

By disabling Camel's 'streamCache' we can prove Camel fails to handle large streams. Camel attemps to preload the data and throws an "out of memory error".

To replicate the failure, comment out the following property:

	camel.springboot.stream-caching-enabled=false

Then, follow the same commands as the "positive" test case.
When the problem is replicated, Camel throws a `java.lang.OutOfMemoryError`.
