# AWS Step Functions

## What you'll learn
- What Step Functions is and how workflows work
- What a state machine is
- When to use Step Functions vs Lambda alone
- Key exam facts

---

## What is AWS Step Functions?

AWS Step Functions is a **serverless visual workflow orchestration service**. It lets you coordinate multiple AWS services into a sequence of steps — defined as a state machine — without writing complex coordination logic in application code.

Instead of embedding "call Lambda, then check result, then call another service, then wait for approval" logic into application code, you define the entire flow visually and as JSON. Step Functions handles retries, error handling, parallelism, and waits between steps automatically.

---

## Key concepts

**State machine** — The definition of your workflow. It describes the sequence of states (steps), transitions between them, and the logic for decisions and error handling.

**States** — Each step in the workflow. State types include:
- **Task**: Call an AWS service (Lambda, ECS, Glue, SageMaker, etc.)
- **Choice**: Branch based on conditions
- **Wait**: Pause for a specified time or until a callback
- **Parallel**: Run multiple branches simultaneously
- **Map**: Apply the same steps to each item in a list
- **Pass**: Pass input to the next state

**Standard workflows** — Long-running workflows (up to 1 year), exactly-once execution, full history and audit trail. Good for order fulfillment, human approval workflows.

**Express workflows** — High-volume, short-duration workflows (up to 5 minutes). At-least-once execution. Good for IoT data ingestion, high-volume streaming pipelines.

---

## When to use it

**Multi-step business processes** — An order fulfillment workflow: validate order → charge payment → reserve inventory → trigger shipping → send confirmation email. Each step is a Lambda function; Step Functions coordinates the sequence.

**Branching logic** — An insurance claims workflow that routes claims differently based on claim amount, type, and history.

**Human approval steps** — A document approval workflow that sends an email, then waits days (or weeks) for a human response before continuing.

**Parallel processing** — Process multiple items simultaneously (e.g., apply transformations to each file in a batch) and wait for all to complete before the next step.

**Error handling** — Define retries with exponential backoff and catch specific error types to route to alternate paths — without try/catch blocks in Lambda code.

---

## Step Functions vs Lambda alone

| | Step Functions | Lambda alone |
|-|---------------|--------------|
| Multi-step coordination | Built-in, visual | Written in application code |
| Error handling | Automatic retries, catch blocks | Must code manually |
| State tracking | Full execution history | No built-in state |
| Long-running workflows | Up to 1 year | 15 minutes max |
| Visibility | Visual console execution view | CloudWatch logs only |

---

## Exam focus

- Step Functions = **orchestrate multi-step workflows** without writing coordination logic
- Uses a **state machine** defined in JSON (Amazon States Language)
- Built-in support for retries, timeouts, error handling, and parallel execution
- **Standard** workflows: up to 1 year, exactly-once, for long-running business processes
- **Express** workflows: high-volume, short-duration, at-least-once
- Works with Lambda, ECS, Glue, SageMaker, SNS, SQS, and other services as task targets
- Use when the exam describes: "coordinate multiple steps," "workflow," "orchestrate services," "approval process," "retry on failure"

---

## Practice questions

**Q1.** A company has a document processing pipeline: extract text with Textract, analyze sentiment with Comprehend, store results in DynamoDB, and send a notification via SNS. Each step must complete before the next begins, and the system must retry automatically on transient failures. Which service orchestrates this pipeline?

A) Amazon SQS  
B) Amazon EventBridge  
C) AWS Step Functions  
D) Amazon SNS

**Answer: C** — Step Functions is designed to coordinate multi-step workflows exactly like this. It sequences Lambda/service calls, handles retries with configurable backoff, and tracks the state of each execution. SQS and EventBridge route and queue messages but don't orchestrate sequential multi-step workflows with conditional logic. SNS is for fan-out notifications.
