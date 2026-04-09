# Serverless Service Alternatives Report

## CST8917 – Assignment 2

---

## Introduction

Throughout this course we worked with a bunch of Azure serverless services to build and deploy applications. This report looks at what the equivalent services are over on AWS and Google Cloud, and how they stack up against each other. For each Azure service we used, I found the closest match on the other two platforms and compared them across features, integrations, monitoring, and pricing.

---

## 1. Azure Functions vs AWS Lambda vs Google Cloud Functions

### Overview

Azure Functions is Microsoft's serverless compute offering. You write a function, pick a trigger, and Azure handles the rest. AWS Lambda is basically the same idea on Amazon's side, and Google Cloud Functions does the same thing on GCP. All three let you run code without managing servers.

| Aspect | Azure Functions | AWS Lambda | Google Cloud Functions |
|---|---|---|---|
| Triggers | HTTP, Timer, Blob, Queue, Event Grid, Service Bus, Cosmos DB, etc. | API Gateway, S3, DynamoDB, SQS, SNS, EventBridge, Kinesis, etc. | HTTP, Pub/Sub, Cloud Storage, Firestore, Firebase events |
| Language Support | C#, JavaScript, Python, Java, PowerShell, TypeScript | Python, Node.js, Java, Go, .NET, Ruby, custom runtimes | Node.js, Python, Go, Java, .NET, Ruby, PHP |
| Bindings | Input/output bindings to many Azure services (built-in) | No native binding concept, you use the SDK directly | No native bindings, SDK-based integration |
| Max Timeout | 10 min (Consumption), unlimited (Premium/Dedicated) | 15 minutes max | 9 min (1st gen), 60 min (2nd gen) |
| Scaling | Automatic, event-driven | Automatic, concurrency-based | Automatic, request-based |

### Integration Options

Azure Functions plugs right into the Azure ecosystem, including Logic Apps, Event Grid, Service Bus, etc. It also works with Azure DevOps and GitHub Actions for CI/CD.

Lambda integrates tightly with the whole AWS stack. API Gateway, Step Functions, S3, DynamoDB, it's all connected. CI/CD is usually done through CodePipeline or third-party tools.

Cloud Functions works well with other GCP services like Pub/Sub, Firestore, and Cloud Run. For CI/CD you'd typically use Cloud Build.

### Monitoring & Observability

- Azure: Application Insights, Azure Monitor
- AWS: CloudWatch (logs, metrics, alarms), X-Ray for tracing
- GCP: Cloud Logging, Cloud Monitoring, Cloud Trace

### Pricing Model

All three charge per execution and compute time (GB-seconds). They all have a free tier too. Azure and AWS are pretty comparable in pricing. GCP's pricing is similar but the free tier is a bit more generous depending on usage.

### Strengths & Weaknesses

Azure Functions has the nicest developer experience if you're already in the Microsoft ecosystem. The bindings system is genuinely useful and saves a lot of boilerplate. Lambda has the most mature ecosystem and the widest adoption. Cloud Functions is the simplest to get started with but has fewer trigger types compared to the other two.

---

## 2. Durable Functions vs AWS Step Functions vs GCP Workflows

### Overview

Durable Functions is an extension of Azure Functions that lets you write stateful workflows in code, things like function chaining, fan-out/fan-in, and human interaction patterns. AWS Step Functions does something similar using state machines defined in JSON (Amazon States Language). GCP Workflows is Google's answer, using YAML-based workflow definitions.

| Aspect | Durable Functions | AWS Step Functions | GCP Workflows |
|---|---|---|---|
| Workflow Definition | Code-based (C#, JS, Python) | JSON/YAML state machine (ASL) | YAML/JSON syntax |
| Patterns | Chaining, Fan-out/Fan-in, Human interaction, Monitoring, Eternal orchestrations | Chaining, Parallel, Map, Choice, Wait | Sequential, Parallel, Conditional, Subworkflows |
| State Management | Built-in (Azure Storage) | Built-in (managed by AWS) | Built-in (managed by GCP) |
| Max Duration | Unlimited (eternal orchestrations) | Standard: 1 year, Express: 5 min | 1 year |
| Pricing | Based on underlying function executions | Per state transition | Per step executed |

### Integration Options

Durable Functions works with anything Azure Functions can talk to, since orchestrations just call regular functions. Step Functions integrates with over 200 AWS services directly through SDK integrations. GCP Workflows can call any HTTP endpoint and has connectors for GCP services.

### Monitoring & Observability

- Azure: Durable Functions has a built-in status endpoint, plus Application Insights
- AWS: Step Functions has a visual execution history in the console, plus CloudWatch
- GCP: Workflows has execution logs in Cloud Logging and a visual execution view

### Pricing Model

Durable Functions charges based on the underlying function executions and storage. Step Functions charges per state transition ($0.025 per 1,000 transitions for Standard). GCP Workflows charges per step ($0.01 per 1,000 steps for internal, $0.025 for external).

### Strengths & Weaknesses

Durable Functions is great if you want to define workflows in actual code rather than config files, and it feels more natural for developers. Step Functions has the best visual tooling and the broadest service integration. GCP Workflows is the newest and simplest but doesn't have as many features yet.

---

## 3. Azure Logic Apps vs AWS Step Functions vs Google Cloud Workflows

### Overview

Logic Apps is Azure's low-code workflow automation tool. It's designed for integration scenarios, connecting SaaS apps, enterprise systems, and Azure services with a visual designer. AWS doesn't have a perfect 1:1 equivalent, but Step Functions combined with EventBridge is the closest. On GCP, Workflows plus Application Integration covers similar ground.

| Aspect | Azure Logic Apps | AWS Step Functions + EventBridge | GCP Workflows + Application Integration |
|---|---|---|---|
| Design Approach | Visual designer (low-code) | JSON state machine + event routing | YAML workflows + visual integration designer |
| Connectors | 400+ built-in connectors (Office 365, Salesforce, SAP, etc.) | 200+ service integrations, plus EventBridge partners | Fewer native connectors, relies more on HTTP/API calls |
| Use Case | Business process automation, SaaS integration | Service orchestration, event-driven workflows | Cloud-native workflow automation |
| Pricing | Per action execution + connector usage | Per state transition + event bus charges | Per step + connector usage |

### Integration Options

Logic Apps shines here. The connector library is massive. You can hook into Office 365, Dynamics, Salesforce, SAP, and hundreds of other services without writing code. AWS covers a lot through EventBridge partner integrations but it's more developer-oriented. GCP's Application Integration is still growing its connector ecosystem.

### Monitoring & Observability

- Azure: Built-in run history, Azure Monitor, Log Analytics
- AWS: CloudWatch, Step Functions execution history, EventBridge monitoring
- GCP: Cloud Logging, execution history in Workflows console

### Strengths & Weaknesses

Logic Apps is the clear winner for non-developer integration scenarios. The visual designer and connector library are hard to beat. AWS is more powerful for complex orchestration but requires more technical skill. GCP is catching up but the connector ecosystem is still limited compared to the other two.

---

## 4. Azure Service Bus vs Amazon SQS/SNS vs Google Cloud Pub/Sub

### Overview

Azure Service Bus is a fully managed enterprise message broker with queues and topics. On AWS, the equivalent is a combination of SQS (queues) and SNS (pub/sub topics). GCP has Pub/Sub which handles both messaging patterns.

| Aspect | Azure Service Bus | Amazon SQS + SNS | Google Cloud Pub/Sub |
|---|---|---|---|
| Queue Support | Yes (with sessions, dead-letter, scheduling) | SQS (standard and FIFO queues) | Pull subscriptions act like queues |
| Topic/Pub-Sub | Yes (topics with subscriptions and filters) | SNS (topics with subscriptions and filtering) | Yes (topics with subscriptions and filtering) |
| Message Ordering | FIFO with sessions | FIFO queues (SQS) | Ordering keys |
| Max Message Size | 256 KB (Standard), 100 MB (Premium) | 256 KB (SQS), 256 KB (SNS) | 10 MB |
| Dead-Letter Queue | Built-in | Built-in (SQS) | Dead-letter topics |
| Transactions | Yes | No | No |

### Integration Options

Service Bus integrates with Azure Functions, Logic Apps, Event Grid, and most Azure services. SQS/SNS plugs into Lambda, Step Functions, and the broader AWS ecosystem. Pub/Sub works with Cloud Functions, Dataflow, BigQuery, and other GCP services.

### Monitoring & Observability

- Azure: Azure Monitor, Service Bus metrics, diagnostic logs
- AWS: CloudWatch metrics for SQS/SNS, CloudTrail for auditing
- GCP: Cloud Monitoring, Cloud Logging, Pub/Sub metrics

### Pricing Model

Service Bus charges based on operations and tier (Basic, Standard, Premium). SQS charges per million requests, SNS per million publishes plus delivery charges. Pub/Sub charges per data volume (first 10 GB free per month).

### Strengths & Weaknesses

Service Bus has the richest feature set. Sessions, transactions, and scheduled messages are things the others don't fully match. SQS/SNS is battle-tested and simple to use but you need two services to cover what Service Bus does alone. Pub/Sub is elegant and handles high throughput well, plus the larger message size limit is nice.

---

## 5. Azure Event Grid vs Amazon EventBridge vs Google Eventarc

### Overview

Event Grid is Azure's event routing service. It delivers events from Azure services (or custom sources) to handlers like Functions, Logic Apps, or webhooks. EventBridge is the AWS equivalent, and Eventarc is Google's version.

| Aspect | Azure Event Grid | Amazon EventBridge | Google Eventarc |
|---|---|---|---|
| Event Sources | Azure services, custom topics, partner events | AWS services, custom apps, SaaS partners | GCP services, custom sources via Pub/Sub |
| Event Filtering | Subject, event type, advanced filters | Content-based filtering (event patterns) | Attribute-based filtering |
| Delivery | Push-based (webhooks, Functions, queues, etc.) | Push-based (Lambda, SQS, SNS, Step Functions, etc.) | Push-based (Cloud Run, Cloud Functions, Workflows) |
| Schema Registry | Yes | Yes | No (relies on CloudEvents spec) |
| Retry & Dead-Letter | Yes | Yes (with DLQ via SQS) | Yes (via Pub/Sub retry policies) |

### Integration Options

Event Grid connects to pretty much every Azure service and supports CloudEvents standard. EventBridge has the widest SaaS partner integration (Zendesk, Datadog, PagerDuty, etc.). Eventarc is tightly coupled with Cloud Run and Cloud Functions but has fewer third-party integrations.

### Monitoring & Observability

- Azure: Azure Monitor, Event Grid metrics, diagnostic logs
- AWS: CloudWatch metrics, EventBridge archive and replay
- GCP: Cloud Logging, Cloud Monitoring, audit logs

### Pricing Model

Event Grid charges per million operations ($0.60/million). EventBridge charges per million events published ($1.00/million). Eventarc pricing is based on underlying Pub/Sub and Cloud Audit Logs usage.

### Strengths & Weaknesses

Event Grid is straightforward and cheap. EventBridge has the best third-party integration story and the archive/replay feature is really useful for debugging. Eventarc is the simplest but also the most limited, it's still relatively new.

---

## 6. Azure Event Hubs vs Amazon Kinesis vs Google Cloud Pub/Sub (Streaming)

### Overview

Event Hubs is Azure's big data streaming platform, think real-time event ingestion at massive scale. Kinesis is the AWS equivalent (specifically Kinesis Data Streams). On GCP, Pub/Sub handles streaming use cases too, though it's architecturally different.

| Aspect | Azure Event Hubs | Amazon Kinesis Data Streams | Google Cloud Pub/Sub |
|---|---|---|---|
| Throughput | Millions of events/sec | Millions of records/sec (with enough shards) | Millions of messages/sec |
| Partitioning | Partition-based | Shard-based | No partitioning (automatic scaling) |
| Retention | 1-90 days (depending on tier) | 1-365 days | 7 days (default), up to 31 days |
| Consumer Groups | Yes | Yes (enhanced fan-out) | Subscriptions act as consumer groups |
| Capture/Archive | Event Hubs Capture (to Blob/Data Lake) | Kinesis Data Firehose (to S3, Redshift, etc.) | BigQuery subscriptions, Cloud Storage export |
| Protocol Support | AMQP, Kafka, HTTPS | AWS SDK, KPL/KCL | gRPC, REST |

### Integration Options

Event Hubs works with Stream Analytics, Azure Functions, Spark, and has Kafka compatibility which is a big deal for migration scenarios. Kinesis integrates with Lambda, Firehose, Analytics, and the broader AWS data stack. Pub/Sub connects to Dataflow (Apache Beam), BigQuery, and Cloud Functions.

### Monitoring & Observability

- Azure: Azure Monitor, Event Hubs metrics, diagnostic logs
- AWS: CloudWatch, Kinesis metrics, enhanced monitoring
- GCP: Cloud Monitoring, Pub/Sub metrics, subscription-level monitoring

### Pricing Model

Event Hubs charges by throughput units and ingress events. Kinesis charges per shard hour plus per-record charges. Pub/Sub charges by data volume. For high-throughput scenarios, Kinesis tends to be the most expensive because of the shard-based model.

### Strengths & Weaknesses

Event Hubs has great Kafka compatibility which makes migration easier. Kinesis is powerful but the shard management can be annoying, you have to think about capacity planning. Pub/Sub is the easiest to use since it auto-scales without partition/shard management, but you get less control over ordering and partitioning.

---

## Summary Comparison Table

| Azure Service | AWS Equivalent | GCP Equivalent |
|---|---|---|
| Azure Functions | AWS Lambda | Google Cloud Functions |
| Durable Functions | AWS Step Functions | GCP Workflows |
| Azure Logic Apps | Step Functions + EventBridge | Workflows + Application Integration |
| Azure Service Bus | Amazon SQS + SNS | Google Cloud Pub/Sub |
| Azure Event Grid | Amazon EventBridge | Google Eventarc |
| Azure Event Hubs | Amazon Kinesis Data Streams | Google Cloud Pub/Sub |

---

## Conclusion

Each cloud provider has a solid serverless offering, but they all have different strengths. Azure has the most cohesive ecosystem if you're already using Microsoft tools — the bindings in Functions and the connector library in Logic Apps are genuinely time-saving. AWS has the most mature and battle-tested services with the widest adoption, which means more community resources and third-party support. GCP tends to be the simplest to get started with and has some elegant design choices (like Pub/Sub's automatic scaling), but the ecosystem isn't as deep as the other two yet.

For most use cases, you could build equivalent architectures on any of the three platforms. The choice usually comes down to what ecosystem you're already invested in and what specific features matter most for your project.
