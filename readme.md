### Understanding publisher and message broker

**a. How much data your publisher program will send to the message broker in one run?**
In this case, the publisher program will send 5 individual data events (UserCreatedEventMessage) to the message broker in a single run.

**b. The url of: "amqp://guest:guest@localhost:5672" is the same as in the subscriber program, what does it mean?**
It basically means that the publisher is connecting to the exact same RabbitMQ message broker instance running locally on port 5672. This is required because both the publisher and subscriber need to be communicating with the identical broker to send and receive the shared events.