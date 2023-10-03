Introduction
============

This project contains logic to proxy back the HTTP response data stream from the server system. The aim is to test Camel's ability to handle large HTTP data streams.


Running a positive test
=======================

To demonstrate Camel can handle larger data streams than memory allocated to the JVM, we need to start it with low memory settings.

To run it follow the commads below:

1. Make sure the server (server-spring-boot) is running.

2. Package the project in a JAR file:

	./mvnw clean package


3. Run the JAR file with low memory settings:

	java -jar -Xms200m -Xmx200m target/demo-1.0.0.jar


4. From the client folder send a download request (read client instructions).
   IMPORTANT: use port 9000 to send the request.


5. Stop the engine and inspect the result file.

  The proxy Camel Spring Boot instance should channel back the HTTP byte stream response from the server. The client should dump the data (output) in a file in the 'data' directory.


Running a negative test
=======================

By disabling Camel's 'streamCache' we can prove Camel fails to handle large streams. Camel attemps to preload the data and throws an "out of memory error".

To replicate the failure, change the parameter `disableStreamCache` as follows:

	from:
        <to uri="http://localhost:8080/test?disableStreamCache=true"/>

	to:
        <to uri="http://localhost:8080/test?disableStreamCache=false"/>


Then, follow the same commands as the "positive" test case.
When the problem is replicated, Camel throws a `java.lang.OutOfMemoryError`.
