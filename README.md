# MegaQueue

**MegaQueue** is a service within [**Mega microservices architecture**](https://github.com/MegaGera/Mega).

*MegaQueue* is a message queue service that handles asynchronous communication between *Mega* services using **RabbitMQ**. It provides reliable message queuing, routing, and management capabilities for the entire Mega ecosystem.

## Table of Contents

- [Service Description](#service-description)
  - [Message Queuing](#message-queuing)
  - [Management Interface](#management-interface)
  - [Configuration](#configuration)
- [Part of Mega](#part-of-mega)
- [Architecture](#architecture)
- [Environment Variables](#environment-variables)
- [Development Server](#development-server)
- [Build & Deploy](#build--deploy)
- [Usage](#usage)
- [License](#license)
- [Contact & Collaborate](#contact--collaborate)

## Service Description

This service provides **message queuing infrastructure** for the Mega microservices architecture.

### Message Queuing

*MegaQueue* uses **RabbitMQ** to provide reliable message queuing between services. It handles:

- **Asynchronous Communication**: Services can send messages without waiting for immediate responses
- **Message Persistence**: Messages are stored until processed, ensuring no data loss
- **Load Balancing**: Distributes messages across multiple consumers
- **Dead Letter Queues**: Handles failed message processing
- **Message Routing**: Routes messages to appropriate queues based on routing keys

**Supported Message Patterns:**
- **Point-to-Point**: Direct message delivery to specific consumers
- **Publish/Subscribe**: Broadcasting messages to multiple subscribers
- **Request/Reply**: Synchronous request-response patterns
- **Work Queues**: Distributing tasks across multiple workers

### Management Interface

*MegaQueue* includes the **RabbitMQ Management Plugin** providing:

- **Web-based Management UI**: Accessible at port 15672
- **Queue Monitoring**: Real-time queue statistics and health
- **Message Inspection**: View and manage queued messages
- **Connection Management**: Monitor active connections
- **Performance Metrics**: Throughput and latency statistics

### Configuration

The service is highly configurable through:

- **RabbitMQ Configuration**: Custom settings in `config/rabbitmq.conf`
- **Environment Variables**: Database credentials and port settings
- **Docker Configuration**: Easy deployment and scaling
- **Network Integration**: Seamless integration with Mega network

## Part of Mega

MegaQueue is part of the larger [**Mega**](https://github.com/MegaGera/Mega) project, a collection of web applications built with a **microservices architecture**.

[**Mega**](https://github.com/MegaGera/Mega) includes other services such as a [Proxy (*MegaProxy*)](https://github.com/MegaGera/MegaProxy), an [Authentication service (*MegaAuth*)](https://github.com/MegaGera/MegaAuth), a [Football App (*MegaGoal*)](https://github.com/MegaGera/MegaGoal), a [Logging service (*MegaLog*)](https://github.com/MegaGera/MegaLog), and other Web Applications ([*MegaMedia*](https://github.com/MegaGera/MegaMedia), [*MegaHome*](https://github.com/MegaGera/MegaHome), [*MegaDocu*](https://docusaurus.io/))

## Architecture

MegaQueue is built on **RabbitMQ** with the following components:

### Core Components
- **RabbitMQ Server**: Message broker and queue management
- **Management Plugin**: Web-based administration interface
- **Configuration Files**: Custom RabbitMQ settings
- **Docker Integration**: Containerized deployment

### Network Integration
- **Mega Network**: Connected to the main Mega Docker network
- **Port Exposure**: AMQP (5672) and Management (15672) ports
- **Volume Persistence**: Data persistence across container restarts
- **Health Checks**: Automated health monitoring

## Environment Variables

In: `.env`

```javascript
# RabbitMQ Admin Credentials
RABBITMQ_DEFAULT_USER=string
RABBITMQ_DEFAULT_PASS=string

# RabbitMQ Configuration
RABBITMQ_DEFAULT_VHOST=string

# Ports (change if needed to avoid conflicts)
AMQP_PORT=number
MANAGEMENT_PORT=number
```

## Development Server

Run `docker-compose up` to start the MegaQueue service in development mode.

The service will be available at:
- **AMQP Port**: 5672 (for message queuing)
- **Management UI**: http://localhost:15672 (for administration)

## Build & Deploy

[`docker-compose.yml`](docker-compose.yml) file manages the RabbitMQ container and handles it easily within the *Mega* network.

The service uses the official **RabbitMQ Docker image** with management plugins enabled.

## Usage

### Connecting to MegaQueue

Services can connect to MegaQueue using the AMQP protocol:

```javascript
// Example connection string
const connectionString = 'amqp://admin:password@megaqueue:5672';

// Using amqplib (Node.js)
const amqp = require('amqplib');
const connection = await amqp.connect(connectionString);
```

### Common Queue Operations

**Publishing Messages:**
```javascript
const channel = await connection.createChannel();
await channel.assertQueue('logging');
channel.sendToQueue('logging', Buffer.from(JSON.stringify(message)));
```

**Consuming Messages:**
```javascript
const channel = await connection.createChannel();
await channel.assertQueue('logging');
channel.consume('logging', (message) => {
  // Process message
  console.log(message.content.toString());
  channel.ack(message);
});
```

### Management Interface

Access the RabbitMQ Management UI at `http://localhost:15672` (development) or `https://megaqueue.megagera.com` (production) to:

- Monitor queue health and statistics
- Inspect messages in queues
- Manage exchanges and bindings
- View connection and channel information
- Configure users and permissions

## License

This project is licensed under the GPL-3.0 License. See the LICENSE file for details.

## Contact & Collaborate

Contact with me to collaborate :)

- gera1397@gmail.com
- GitHub: [MegaGera](https://github.com/MegaGera)
