# AWS IoT Core

## What you'll learn
- What IoT Core is and what it connects
- How the message broker and rules engine work
- Key use cases
- Key exam facts

---

## What is AWS IoT Core?

AWS IoT Core is a **managed cloud platform that lets connected devices interact with AWS services and other devices**. It acts as the central hub for IoT device communication — securely receiving messages from millions of devices and routing that data to AWS services for processing, storage, and analysis.

---

## How IoT Core works

**Device connection** — Devices connect to IoT Core using MQTT (a lightweight publish/subscribe protocol designed for constrained devices), HTTP, or WebSocket. Each device authenticates using X.509 certificates.

**Message broker** — IoT Core is essentially a large-scale MQTT message broker. Devices publish messages to topics (e.g., `factory/machine-3/temperature`). Other services or devices subscribed to those topics receive the messages.

**Rules engine** — The rules engine evaluates incoming messages using SQL-like queries and routes matching messages to AWS services. For example: "If temperature > 80, store in DynamoDB and send an SNS alert."

**Device Shadow** — A JSON document that stores the current and desired state of a device. If a device goes offline, commands sent to it are stored in the shadow and applied when it reconnects.

---

## Key integrations via the Rules Engine

| Target | Use case |
|--------|---------|
| Amazon DynamoDB | Store sensor readings as time-series data |
| Amazon S3 | Archive raw device data |
| Amazon Kinesis | Stream high-volume telemetry for real-time analysis |
| AWS Lambda | Trigger serverless functions on device events |
| Amazon SNS | Alert operations teams on threshold violations |
| Amazon CloudWatch | Monitor device metrics |

---

## Key features

- **Scale**: Connects billions of devices with low latency
- **Security**: TLS encryption in transit; X.509 certificates for device authentication; IAM policies for service access
- **Device Shadow**: Virtual representation of device state for offline devices
- **Rules Engine**: Route messages to AWS services without writing routing code
- **Managed infrastructure**: No message broker servers to manage

---

## When to use it

**Industrial IoT** — Collect sensor data from factory machines, monitor equipment health, trigger maintenance alerts.

**Connected vehicles** — Receive telemetry from vehicle fleets, track location and diagnostics.

**Smart home / smart building** — Connect thermostats, locks, lighting, and sensors to cloud dashboards and automation.

**Agriculture** — Soil sensors, irrigation controllers, and weather stations sending data to analytics pipelines.

---

## Exam focus

- IoT Core = **managed device gateway and message broker** for IoT devices
- Devices connect via **MQTT**, HTTP, or WebSocket
- **Rules Engine** routes messages to DynamoDB, Lambda, Kinesis, SNS, S3, and other services
- **Device Shadow** stores device state for offline devices
- Use when exam describes: "connect IoT devices to AWS," "process sensor data," "millions of devices sending telemetry"

---

## Practice questions

**Q1.** A manufacturing company has thousands of factory sensors that send temperature readings every 10 seconds. They need to store all readings in DynamoDB and send an SMS alert when any reading exceeds a threshold. Which AWS service handles the device connections and message routing?

A) Amazon Kinesis  
B) Amazon SNS  
C) AWS IoT Core  
D) Amazon SQS

**Answer: C** — IoT Core acts as the managed MQTT broker that receives messages from all sensors. Its Rules Engine evaluates incoming messages and routes them to DynamoDB for storage and SNS for alerting — all with a few rules, no custom routing code. Kinesis is for streaming data analytics (could be used downstream). SNS handles notifications but cannot receive device messages. SQS is a message queue, not a device gateway.
