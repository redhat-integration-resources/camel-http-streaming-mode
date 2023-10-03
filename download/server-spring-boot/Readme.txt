Introduction
============

This project contains logic to accept download requests (via HTTP). The server reads a large file from the file system and send the data stream back to the client. The aim is to test Camel's ability to handle large HTTP data streams (download).


Running a positive test
=======================

To demonstrate Camel can handle larger data streams than memory allocated to the JVM, we need to start it with low memory settings.

To run it follow the commads below:

1. Package the project in a JAR file:

	./mvnw clean package


2. Run the JAR file with low memory settings:

	java -jar -Xms200m -Xmx200m target/demo-1.0.0.jar


3. From the client folder send a request (follow client instructions).


4. Stop Camel Spring Boot and inspect the result file.

  Camel Spring Boot should process the HTTP request and send back the byte stream. The client dumps the data (output) in a file in the 'data' directory.


Running a negative test
=======================

By switching Camel's file reader we can prove Camel fails to handle large streams. Camel attemps to preload the data and throws an "out of memory error".

To replicate the failure:

1) Comment out the following code block:

	<pollEnrich>
      ...
    </pollEnrich>

2) Uncomment the following line:

    <to uri="language:constant:resource:file:../data/input"/>


Then, follow the same commands as the "positive" test case.
When the problem is replicated, Camel throws a `java.lang.OutOfMemoryError`.