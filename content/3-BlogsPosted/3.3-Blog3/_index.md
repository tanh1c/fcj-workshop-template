---
title: "One safe action does not necessarily make a safe sequence"
date: 2026-07-21
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

## AI Agent Safety | One safe action does not necessarily make a safe sequence

| Information | Details |
|---|---|
| Publication date | 21/07/2026 |
| Status | Published |
| Platform | AWS Study Group - Facebook Group |
| Published post | [View the post on Facebook](https://www.facebook.com/groups/awsstudygroupfcj/?multi_permalinks=2220015812096712) |
| Topic | AI agent safety, trajectory analysis, and sandboxed code execution |

Hello everyone,

When evaluating the safety of an AI agent, we often examine the prompt, the final answer, or each individual tool call.

But suppose an agent performs three steps:

1. Reads an internal file.
2. Summarizes its contents.
3. Sends the summary to an external service.

Each action may appear legitimate in isolation, while the complete sequence creates a risk of data leakage.

This is why some recent research moves from evaluating individual actions to analyzing the entire **trajectory** — the history of reasoning, tool calls, and responses from the environment.

ATBench constructs 1,000 safe and unsafe trajectories to test risk detection in multi-step interactions. HINTBench further indicates that a model may recognize that a session is becoming dangerous while still struggling to identify the exact action where the problem began.

This suggests that filtering prompts or blocking a few sensitive commands is not enough. An agent protection system should combine:

- Restrictions on permissions and available tools.
- Code execution in an isolated environment.
- Approval checks before critical operations.
- Complete trajectory logging to detect accumulated risk.

Amazon Bedrock AgentCore Code Interpreter, for example, supports code execution in an isolated container environment and network modes such as Sandbox, Public, and VPC. However, a sandbox only limits consequences; it does not determine whether the agent is moving toward the wrong objective.

![AI agent tool execution flow with AgentCore Code Interpreter](/images/blogs/blog3-agentcore-code-interpreter-flow.webp)

*Figure 1. AI agent tool execution flow with a Code Interpreter session and observability telemetry.*

My takeaway is that AI agent safety is no longer only about controlling **what the agent says**. It also requires observing **what the agent has done and where the sequence of actions is leading**.

When deploying an AI agent, do you think teams should prioritize a strict sandbox from the beginning, or invest more heavily in trajectory monitoring?

## References

- [ATBench](https://arxiv.org/abs/2604.02022)
- [HINTBench](https://arxiv.org/abs/2604.13954)
- [Amazon Bedrock AgentCore Code Interpreter](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/code-interpreter-tool.html)

---

[Previous](/3-blogsposted/3.2-blog2/) | [Back to Blogs Posted](/3-blogsposted/) | [Next](/3-blogsposted/3.4-blog4/)
