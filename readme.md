### Understanding publisher and message broker

**a. How much data your publisher program will send to the message broker in one run?**
In this case, the publisher program will send 5 individual data events (UserCreatedEventMessage) to the message broker in a single run.

**b. The url of: "amqp://guest:guest@localhost:5672" is the same as in the subscriber program, what does it mean?**
It basically means that the publisher is connecting to the exact same RabbitMQ message broker instance running locally on port 5672. This is required because both the publisher and subscriber need to be communicating with the identical broker to send and receive the shared events.


### RabbitMQ Dashboard
![RabbitMQ Dashboard](RabbitMQ.png)


### Sending and processing event
![Processes](SendingProcesses.png)
This screenshot demonstrates the asynchronous decoupling provided by the message broker. On the right, the Publisher is executed multiple times in rapid succession, instantly firing off batches of events to RabbitMQ and completing its process. On the left, the single 'slow' Subscriber continuously works through the resulting backlog of messages at its own pace (with a 1-second delay). The system remains stable because RabbitMQ safely buffers all incoming events in the queue, ensuring no data is lost even when the producer vastly outpaces the consumer.

### Spike Monitor
![Spike 1](Spike.png)
![Spike 2](Spike2.png)
The spike in the RabbitMQ 'Message rates' chart demonstrates the message broker acting as a buffer between an asynchronous publisher and subscriber. Because the Publisher sent all 5 UserCreatedEventMessage events almost instantly, but the Subscriber was artificially delayed (thread::sleep for 1 second per message) to simulate a slow worker, the messages could not be processed immediately. RabbitMQ temporarily stored these unacknowledged messages in its queue, basically causing the visible spike in the graph—and then steadily fed them to the Subscriber one by one until the queue was empty

### Multiple Subscriber
![Terminal](Multiple.png)
![RabbitMQ](MultipleRabbitMQ.png)
When scaling up by running multiple subscriber instances simultaneously, the queue was processed much faster. RabbitMQ distributed the workload across the available subscribers using a round-robin approach, preventing a bottleneck.