# <center>Background Jobs</center>

Background Jobs also known as asynchronous tasks or jobs, are a common technique in software development for handling tasks that can be executed independently of the main user interactions or request-response cycle.

<p>These tasks are typically performed in the "background", separate from the immediate user experience. Background jobs are used to improve system responsiveness, handle time-consuming tasks, and offload resource-intensive operations from the main application thread or process.</p>

**Example**: Data processing, file uploads, sending emails, generating reports or performing system maintenance.

### Key Concepts & Benefits of Background jobs

##### 1. Asynchronous Execution:

Background jobs are executed asynchronously, meaning they are started and managed independently of the main execution flow.

##### 2. Queueing Systems:

Many background jobs are managed using queueing systems. These systems prioritize, schedule, and distribute tasks to works that execute them.

<p>Examples of queueing systems include RabbitMQ, Apache Kafka, Redis with its built-in queueing features.</p>

##### 3. Worker Processes:

Worker processes are responsible for executing background jobs. They consume tasks from the queue and perform the required operations. Workers can run on separate machines or be part of a distributed setup.

##### 4. Fault Tolerance and Retry Mechanisms:

Background job systems often include built-in mechanisms to handle failures. If a job fails to execute, it can be retried a certain number of times before being marked as failed.

##### 5. Delayed Execution:

Background jobs can be scheduled for delayed execution.

##### 6. Batch Processing:

Background jobs are often used for batch processing tasks, such as bulk data import, data transformation, or report generation.

##### 7. Long-Running Tasks:

Some tasks, such as video transcoding or machine learning model training, can take a long time to complete. Background jobs allow these tasks to be processed without impacting the responsiveness of the main application.

##### 8. Monitoring and Reporting:

Background job systems often provide monitoring tools and dashboards to track the status and progress of running jobs.

---

```code
        ________Background Jobs________
       |                               |
Scheduled-driven                  Event-Driven
```

## 1. Scheduled Driven

"**Scheduled-driven**" refers to a type of process or task execution that is triggered or initiated based on a predefined schedule or time interval.

<p>scheduled-driven tasks are often used to automate repetitive actions, maintenance tasks, data synchronization, and other operations that need to occur at specific times or intervals. These tasks are typically implemented using scheduling mechanisms and background job processing.</p>

Popular tools include `cron jobs` (for Unix-like systems), `Windows Task Scheduler` (for Windows systems), and `cloud-based scheduling` services.</p>

**Examples** of scheduled-driven tasks include:

- A daily backup of a database.
- Sending a weekly email newsletter to subscribers.
- Clearing cache files every hour.
- Updating stock prices from external sources every minute.
- Running a batch process to calculate monthly sales reports.
- Performing system health checks every 10 minutes.

## 2. Event Driven

"**Event-driven**" architecture is a design approach in software development where the flow of a system is determined by events or messages that are produced, consumed, and processed by different components or services.

<p>In event-driven systems, components are decoupled and interact through events, enabling loosely-coupled, scalable, and flexible architectures.</p>

<p>This approach is commonly used in various types of applications, including microservices, real-time systems, and user interfaces.</p>

To implement event-driven architectures, various technologies and tools are available, including message brokers like `RabbitMQ`, `Apache Kafka`, and cloud-based event hubs.

Here are some key concepts and benefits of event-driven architecture:

#### A. Events:

An event is a signal or notification that something has occurred in the system.

#### B. Publish-Subscribe Pattern:

In event-driven architecture, the `publish-subscribe` pattern is often used.

- **Publishers** generate events and send them to a message broker or event bus.
- **Subscribers** register their interest in certain types of events and receive notifications when those events occur.

#### C. Loose Coupling:

Components in an event-driven architecture are decoupled, meaning they don't need to know the details of each other's implementations. This allows for easier maintenance, scalability, and changes to individual components without affecting the entire system.

#### D. Flexibility:

Event-driven systems are adaptable and flexible. New features or services can be introduced by simply adding new event producers and consumers.

#### E. Real-Time Processing:

Well-suited for real-time and reactive systems that need to respond quickly to changing conditions or user interactions.

#### F. Asynchronous Processing:

Events are processed asynchronously, allowing components to perform tasks without waiting for immediate responses.

#### G. Event Sourcing and CQRS:

Event-driven architectures are often used in combination with `event sourcing` and `Command Query Responsibility Segregation` **CQRS**

<p>which enable storing and processing events to reconstruct the state of the system and optimize read and write operations.</p>

- [Event Sourcing](https://www.geeksforgeeks.org/system-design/event-sourcing-pattern/) records every change as an event instead of just storing the current state, creating a complete history of data changes. The current state can be rebuilt anytime by replaying these events, helping track how data evolved over time.
- [Command Query Responsibility Segregation (CQRS)](https://newsletter.scalablethread.com/p/what-is-command-query-responsibility) is an architectural pattern that separates an application's write operations (commands) from its read operations (queries) into distinct models or paths.

#### 8. Complex Workflows:

Event-driven architectures can handle complex workflows and interactions between components, allowing for the coordination of various actions across the system.

**Examples** of event-driven architecture include:

1. **microservices-based** e-commerce platform where different services communicate through events for order processing, inventory updates, and payment notifications.
2. **Internet of Things (IoT)** applications where sensor readings trigger events for data analysis, alerts, and automation.
3. User interface interactions, such as updating a dashboard in real-time when data changes.
4. Financial systems that react to market data changes and trigger automated trading actions.

---

# <center> Returning Results </center>

<p>Returning results in a software context refers to the process of providing output or responses to users, clients, or other components after a request or task has been processed.</p>

<strong>The way results are returned depends on the nature of the application, the communication protocol being used, and the specific requirements of the system.</strong>

### 1. Synchronous Response:

In a **synchronous response** model, the requester waits for a response from the system before continuing its operations. This is common in traditional request-response interactions. For example, when you make an HTTP request to a web server, the server processes the request and sends back a response with the result (e.g., a web page or data).

### 2. Asynchronous Response:

In an **asynchronous response** model, the requester doesn't wait for an immediate response. Instead, the system acknowledges the request and processes it in the background. The requester might later check for the results or receive a notification when the results are available. Asynchronous responses are often used in long-running tasks or when immediate results are not critical.

### 3. Callback Functions:

In programming, callback functions are used to handle asynchronous responses. Instead of waiting for the result, the requester provides a callback function that the system will invoke when the result is ready.

```code
A callback function is a function which is:
accessible by another function, and
is invoked after the first function if that first function completes

A callback function is a function passed as an argument into another function.
The callback function is intended to be executed later.

Callbacks are often used in JavaScript, especially in event handling.
User interactions, such as button clicks or key presses, can be handled by providing a callback function to an event listener:
```

### 4. Webhooks:

Webhooks are a way to receive asynchronous notifications from external systems. When an event occurs in a remote system, it sends an HTTP request to a predefined URL (the webhook), allowing the system to process the event and return a response.

<P>A webhook is a lightweight, event-driven communication that automatically sends data between applications via HTTP. Triggered by specific events, webhooks automate communication between application programming interfaces (APIs) and can be used to activate workflows, such as in GitOps environments.</P>

![polling vs webhooks](https://cdn.shopify.com/shopifycloud/shopify-dev/production/assets/assets/images/apps/how-webhooks-work-Dhi08wMg.png)

### 5. Push Notifications:

Push notifications are used to deliver real-time updates or information to users' devices or applications. They're often used in mobile apps to alert users about new messages, updates, or events.

![push notification system](https://assets.bytebytego.com/diagrams/0042-design-a-notification-push-system.png)

### 6. Streaming:

Streaming is used to provide continuous, real-time updates to clients. It's common in scenarios where data is constantly changing, such as live feeds or financial data.

### 7. Batch Processing:

For tasks that involve processing large amounts of data, results might be returned as batches. The system processes data in chunks and then returns the results in bulk.

### 8. Distributed Systems:

In distributed systems, results might be returned through messaging systems, message queues, or event buses. This allows components to communicate and exchange results across different parts of the system.
