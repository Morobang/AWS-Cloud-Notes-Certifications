# Internet of Things (IoT) — Category Overview

AWS IoT services connect physical devices to the cloud — enabling sensor data ingestion, device management, and real-time processing at scale.

---

## Services in this category

| Service | Purpose | Key concept |
|---------|---------|-------------|
| AWS IoT Core | Managed message broker and device gateway for IoT devices | Securely connect billions of devices to AWS |

---

## What makes IoT different

IoT architectures deal with:

- **Massive device counts** — Thousands to billions of physical devices
- **Intermittent connectivity** — Devices connect, send data, and disconnect
- **Constrained devices** — Low power, limited CPU/memory
- **High-volume telemetry** — Sensor readings arriving continuously at scale

Standard application architectures (EC2 + RDS) don't handle these patterns well. IoT Core is designed specifically for device connectivity, message routing, and integration with AWS data services.

---

## Exam selection guide

- "Connect IoT sensors to AWS for data processing" → **AWS IoT Core**
- "Receive telemetry from millions of devices and store in DynamoDB" → **AWS IoT Core**
- "Manage and monitor a fleet of connected devices" → **AWS IoT Core**
