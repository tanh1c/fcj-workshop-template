---
title: "Should the entire deployment be rolled back when a new feature fails?"
date: 2026-07-22
weight: 5
chapter: false
pre: " <b> 3.5. </b> "
---

## AWS DEVOPS | Should the entire deployment be rolled back when a new feature fails?

| Information | Details |
|---|---|
| Publication date | 22/07/2026 |
| Status | Published |
| Platform | AWS Study Group - Facebook Group |
| Published post | [View the post on Facebook](https://www.facebook.com/groups/awsstudygroupfcj/?multi_permalinks=2221213298643630) |
| Topic | Feature flags, AWS AppConfig, AWS DevOps Agent, LaunchDarkly, and post-release incident response |

Hello everyone,

Many of you have probably experienced this situation: immediately after a deployment, the error rate increases, an API begins to slow down, or a feature stops working.

The most common response is to roll back immediately to the previous version.

This approach is safe, but it can sometimes be excessive. If a deployment contains ten changes and only one new feature causes the problem, rolling back the entire deployment also removes the other nine changes that are working normally.

In this situation, a **feature flag** may be a more suitable option.

A feature flag can be understood simply as a switch that turns a feature on or off:

```text
new_checkout = ON / OFF
```

When the new checkout flow has a problem, the operations team only needs to turn off the flag so that users return to the old flow, without rebuilding and redeploying the entire application.

A possible incident-response process is:

```text
CloudWatch detects an issue
        ↓
Inspect recently released changes
        ↓
Identify the related feature flag
        ↓
Turn off the flag
        ↓
Monitor error rate and latency
```

AWS AppConfig supports feature flag management, gradual deployment to groups of users, and CloudWatch alarm monitoring. If an alarm is triggered during a rollout, AppConfig can automatically return the configuration to its previous version.

AWS has also introduced an integration between AWS DevOps Agent and LaunchDarkly. During an incident investigation, the agent can retrieve flag states, targeting rules, and rollout percentages to provide recommendations to engineers. The important point is that the agent supports information gathering and recommendations; it does not necessarily have to disable a feature on its own.

![Full deployment rollback compared with feature flag incident response](/images/blogs/blog5-feature-flag-incident-response.webp)

*Figure 1. Comparison between rolling back an entire deployment and disabling only the affected feature, followed by a controlled incident-response workflow using CloudWatch, AWS DevOps Agent, and LaunchDarkly.*

However, a feature flag is not a button that can “rescue every incident.”

If the cause is in the database, network, or an external service, turning off a flag may not solve anything. Using too many flags without cleaning them up also creates more branches in the code and makes testing increasingly difficult.

In my view, a feature flag is most useful when treated as a **controlled emergency switch**: it reduces the impact on users first, while the root cause still needs to be found and fixed afterward.

When production encounters an issue immediately after a release, do you usually roll back the entire version or disable only the affected feature first?

## References

- [AWS DevOps Blog – Feature flag orchestration with AWS DevOps Agent and LaunchDarkly](https://aws.amazon.com/blogs/devops/feature-flag-orchestration-with-aws-devops-agent-and-launchdarkly/)
- [AWS AppConfig – What is AWS AppConfig?](https://docs.aws.amazon.com/appconfig/latest/userguide/what-is-appconfig.html)

---

[Previous](/3-blogsposted/3.4-blog4/) | [Back to Blogs Posted](/3-blogsposted/)
