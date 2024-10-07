
# Email Scheduler

Email scheduler is a job scheduler that sends future emails using the Master/Worker pattern.

## Basic Architecture

- Task Creation:
    * Insert a document in task_queue DB:
    (recipient_email, subject, body, due, status=QUEUED, worker)
- Task Processing:
    * Workers process QUEUED tasks, send emails, and mark status=SENT.

## Resiliency

* Multiple Workers: Redundant workers reduce single points of failure.
* Heartbeat Checks:
    * Master to Workers: Reassigns tasks if a worker fails.
    * Workers to Master: If the master fails, workers compe to claim the role via an atomic registry update.
* Retry Mechanism: Exponential backoff and max_retry limit for task failures.

## Features

* Distributed & Fault Tolerant: Master/Worker failover ensures reliability.
* Scalable: Easily add workers to handle increased load.
* Task Retry: Failed tasks are retried with backoff strategy.

    