---
title: "Why can a web deployment still cause 502 errors?"
date: 2026-07-22
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

## AWS DEVOPS | Why can a web deployment still cause 502 errors?

| Information | Details |
|---|---|
| Publication date | 22/07/2026 |
| Status | Published |
| Platform | AWS Study Group - Facebook Group |
| Published post | [View the post on Facebook](https://www.facebook.com/groups/awsstudygroupfcj/?multi_permalinks=2220504978714462) |
| Topic | Amazon ECS deployment safety, graceful shutdown, load balancer connection draining, and rollback |

Hello everyone,

A frustrating situation when operating a web application is this: the new version has been deployed successfully and the new container is healthy, but during the few seconds of transition, some users still encounter `502` errors, interrupted requests, or unfinished tasks that are lost.

Sometimes the cause is not the new code, but how the old container is stopped.

Suppose an application is running on Amazon ECS and receiving traffic through an Application Load Balancer:

```text
User
  ↓
Application Load Balancer
  ↓
Old ECS Task + New ECS Task
```

During deployment, ECS starts a new task and gradually stops the old one. However, the old task may still be processing a long-running request, maintaining a database connection, or running an unfinished job.

If the process is terminated immediately, those requests will be interrupted midway.

![Amazon ECS rolling deployment lifecycle with old and new tasks](/images/blogs/blog4-ecs-deployment-lifecycle.webp)

*Figure 1. Amazon ECS rolling deployment lifecycle while traffic transitions from the old task to the new task.*

## A green health check does not mean the deployment is safe

A health check primarily answers this question:

> Is the new container ready to receive requests?

It does not guarantee that the old container has finished processing all requests before it is stopped.

To address this, the Application Load Balancer uses a **deregistration delay**. When a target is removed from a target group, the load balancer stops sending it new requests but gives in-flight requests more time to finish. The current default value is 300 seconds and can be adjusted according to the application’s characteristics.

```text
Stop receiving new requests
          ↓
Wait for in-flight requests to finish
          ↓
Stop the container
```

Setting a very long delay is not always better. If most requests take only a few hundred milliseconds, waiting five minutes can make deployments and scale-in operations unnecessarily slow.

## The application must also know how to shut down

When ECS stops a task, the container first receives a `SIGTERM` signal. If the process does not exit within the allowed time, ECS sends `SIGKILL` to force it to stop. The default waiting period is 30 seconds and can be configured with `stopTimeout`.

The application should handle `SIGTERM` by:

- Stopping the acceptance of new requests.
- Completing requests that are already running.
- Closing database connections.
- Flushing logs or data still held in memory.
- Exiting the process only after those steps are complete.

If the application does not implement graceful shutdown, increasing the deregistration delay may still not solve the problem.

![Application Load Balancer connection draining and Amazon ECS graceful shutdown flow](/images/blogs/blog4-alb-graceful-shutdown-flow.webp)

*Figure 2. Coordination among Application Load Balancer connection draining, Amazon ECS task stopping, and application graceful shutdown.*

## A new container also needs time to start

The opposite problem occurs when a newly started container is marked unhealthy because the application is still connecting to the database, loading configuration, or warming up its cache.

Amazon ECS provides `healthCheckGracePeriodSeconds`, which temporarily ignores failed health checks while a new task is starting.

However, an excessively long grace period also has a downside: a genuinely faulty container may remain active longer before it is replaced.

## When the new deployment is genuinely broken

The ECS Deployment Circuit Breaker can detect when a deployment fails to reach a steady state and automatically roll it back to the previous deployment.

It is useful when a container cannot start or repeatedly fails health checks. However, the circuit breaker does not replace application-level monitoring.

![Amazon ECS deployment safeguards with health checks monitoring and rollback](/images/blogs/blog4-ecs-deployment-safeguards.webp)

*Figure 3. Amazon ECS deployment safeguards combining startup grace periods, health checks, monitoring, circuit breaker detection, and rollback.*

A new version may still be technically healthy while it:

- Responds more slowly.
- Produces a higher error rate.
- Returns incorrect data.
- Breaks a rarely used feature.

Therefore, a safe deployment still requires monitoring latency, HTTP 5xx responses, error rates, and business metrics after release.

My takeaway is that a “zero-downtime deployment” does not come from a single setting. It requires coordination among the load balancer, ECS scheduler, health checks, and the way the application responds when it is stopped.

Sometimes the system does not fail because of the new version, but because the old version did not have enough time to say goodbye 😄

Do you test graceful shutdown as part of your deployment pipeline, or only discover the problem when production starts dropping requests?

## References

- [Amazon ECS – Optimize load balancer connection draining](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/load-balancer-connection-draining.html)
- [Application Load Balancer – Deregistration delay](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/edit-target-group-attributes.html)
- [Amazon ECS deployment circuit breaker](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/deployment-circuit-breaker.html)

---

[Previous](/3-blogsposted/3.3-blog3/) | [Back to Blogs Posted](/3-blogsposted/) | [Next](/3-blogsposted/3.5-blog5/)
