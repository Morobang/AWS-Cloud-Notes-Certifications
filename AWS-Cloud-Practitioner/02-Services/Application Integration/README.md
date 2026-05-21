# Application Integration Services

Application integration services decouple components of a distributed system so they communicate asynchronously — reducing tight coupling and improving reliability, scalability, and maintainability.

## Services in this category

| Service | What it does | Key use case |
|---------|-------------|--------------|
| [Amazon SNS](./Amazon%20Simple%20Notification%20Service%20%28Amazon%20SNS%29.md) | Pub/sub messaging — one message to many subscribers | Fan-out notifications |
| [Amazon SQS](./Amazon%20Simple%20Queue%20Service%20%28Amazon%20SQS%29.md) | Message queue — one message to one consumer | Decouple producers and consumers |
| [Amazon EventBridge](./Amazon%20EventBridge.md) | Event bus with content-based routing | Event-driven architectures |
| [AWS Step Functions](./AWS%20Step%20Functions.md) | Visual workflow orchestration | Multi-step process coordination |

## How to choose

```
One event must reach multiple services simultaneously → SNS (fan-out)
Smooth traffic spikes, buffer between services → SQS (queue)
Route events based on their content/source → EventBridge
Coordinate a multi-step workflow with decisions → Step Functions
```
