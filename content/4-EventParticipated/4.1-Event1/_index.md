---
title: "Event 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

## Event 1: FiRST CLOUD JOURNEY Community Day

| Field | Information |
|---|---|
| Event name | FiRST CLOUD JOURNEY Community Day |
| Date and time | 09:00, 06/06/2026 |
| Location | 26th Floor, Bitexco Tower, 02 Hai Trieu Street, Saigon Ward, Ho Chi Minh City |
| Role | Attendee |
| Registration purpose | Attend Events |
| Work shift | Fulltime |
| Registered participant | Chu Nguyễn Tuấn Anh |
| University | Trường Đại học Bách khoa |
| Student ID | 2352022 |

## Overview

The **FiRST CLOUD JOURNEY Community Day** was a community learning event that brought together speakers from system administration, cloud-native development, cybersecurity, game development, project teamwork, and artificial intelligence. The event was valuable because it did not focus on only one isolated technology. Instead, it showed how cloud, DevOps, security, GenAI, teamwork, and real-world engineering experience connect in modern technology projects.

As an attendee, I joined the event to learn from practical stories and technical sharing sessions. The sessions helped me broaden my view beyond my internship project and understand how different AWS and cloud-native ideas can be applied in production-like scenarios.

## Speakers and Topics

The event included several speakers with different backgrounds:

| Speaker | Topic Area |
|---|---|
| Trần Trung Vinh | Career journey from IT Helpdesk to Senior Sysadmin, Cloud, and DevOps |
| Trương Huy Phước | Project management mindset and effective teamwork |
| Bảo Huỳnh | Docker and cloud-native application packaging |
| Lê Hoàng Gia Đại | AWS WAF and ML-based Network Intrusion Detection System |
| Nguyễn Quốc Bảo | Multiplayer game development with AWS WebSockets and Godot 4 |
| Việt Phát | GraphRAG with Generative AI, Amazon Bedrock, and Amazon Neptune |

## Key Sessions and Knowledge Gained

### Career Growth from IT Helpdesk to Cloud and DevOps

One of the most practical sessions described the journey from an IT Helpdesk role to Senior Sysadmin, Cloud, and DevOps responsibilities. The main lesson was that a strong technology career can be built through consistent self-learning, hands-on labs, and real operational experience.

The session emphasized several important habits: learning Linux and networking deeply, documenting configurations and runbooks, building monitoring before incidents happen, and avoiding direct testing on production systems. This message was especially relevant to me because my internship project also required careful validation, evidence collection, and cost-aware operation on AWS.

### Effective Teamwork and Project Execution

Another session focused on teamwork efficiency. The speaker explained that individual productivity is different from team productivity, and that a strong team needs shared goals, suitable task assignment, open communication, active listening, and personal accountability.

The four teamwork rules that stood out were:

- Clear and shared goals.
- Right person, right place.
- Open communication and active listening.
- Personal accountability.

This helped me reflect on how internship work is not only about completing technical tasks, but also about communicating progress, aligning expectations, and using tools such as Trello, ClickUp, Google Workspace, Slack, or Discord to coordinate work more effectively.

### Docker and Containerization

The Docker session explained why containerization is important for modern software delivery. Compared with traditional virtual machines, containers are lighter, faster to start, and easier to move across environments. Docker packages an application with its dependencies and configuration, which helps reduce the common problem of software working on one machine but failing on another.

The session also introduced core Docker concepts such as Dockerfile, image layers, build cache, container lifecycle commands, networks, and volumes. This strengthened my understanding of why containers are widely used in CI/CD pipelines, microservices, cloud-native applications, and legacy modernization projects.

### AWS WAF and ML-based Network Intrusion Detection

The cybersecurity session was one of the most technically interesting parts of the event. It explained that AWS WAF is useful for blocking common Layer 7 threats such as SQL Injection, XSS, and bot traffic, but rule-based protection alone may not detect unknown or zero-day behavior.

The proposed approach combined AWS WAF with a Machine Learning based Network Intrusion Detection System. The speaker discussed a workflow using network traffic datasets, feature cleaning, class balancing, and model comparison across algorithms such as Random Forest, LightGBM, MLP, 1D-CNN, and XGBoost. The architecture included AWS services such as WAF, ALB, EC2, VPC, Kinesis Data Firehose, S3, Lambda, SNS, Security Hub, and GuardDuty.

This session connected strongly with my own project because both systems use ML to identify risky behavior. In my project, the risk source is an AI coding-agent trajectory; in this event session, the risk source was network traffic. The shared lesson is that ML-based detection should complement rule-based protection instead of replacing it completely.

### Multiplayer Game Architecture with AWS WebSockets and Godot 4

The game development session introduced how real-time multiplayer features can be implemented using Godot 4 and AWS WebSocket architecture. The speaker compared UDP/ENet, HTTP polling, and WebSocket communication, then explained why WebSocket is suitable for lobby systems, chat, and turn-based games.

The proposed serverless architecture used API Gateway WebSocket routes, AWS Lambda for stateless game logic, and DynamoDB to store player connection state and game progress. Practical issues such as stale connections, `GoneException`, DynamoDB scan cost, and stateless Lambda behavior were discussed.

The main value of this session was understanding trade-offs. Serverless WebSocket can reduce operational burden and cost for certain games, but it requires careful state management and may not fit large-scale, physics-heavy real-time games. For those cases, services such as AWS GameLift would be more suitable.

### GraphRAG with Amazon Bedrock and Amazon Neptune

The AI session introduced GraphRAG, an approach that improves standard Retrieval-Augmented Generation by combining generative AI with knowledge graphs. Traditional RAG often retrieves isolated text chunks, which can be insufficient for questions that require multi-hop reasoning across related entities.

GraphRAG stores entities as nodes and relationships as edges, allowing the system to traverse connections and provide more context-aware answers. The session described two AWS implementation directions: a managed route using Amazon Bedrock Knowledge Bases and Amazon Neptune Analytics, and a custom route using frameworks such as LlamaIndex with Amazon Neptune.

This session gave me a clearer view of how GenAI systems can be improved through better data structure and retrieval design, not only through larger language models.

## Event Experience

The event was memorable because it combined technical depth with real career stories. I was able to listen to speakers from industry and university backgrounds, observe practical demos, and learn about challenges that are often not visible in basic tutorials, such as stale WebSocket connections, database scan cost, Docker layer caching, and ML model evaluation for security problems.

The in-person setting also helped me practice professional participation: arriving at the venue, following the event agenda, listening actively, taking notes, and connecting the shared knowledge back to my own project. The event provided not only knowledge but also motivation and a stronger sense of belonging to the cloud and AI learning community.

## Lessons Learned

- Hands-on practice is more persuasive than theory alone. Building labs, documenting work, and creating a working portfolio are important for career growth.
- Architecture decisions are trade-offs. Containers, virtual machines, serverless WebSocket, dedicated game servers, rule-based security, and ML-based detection each fit different contexts.
- Security should combine prevention and detection. Rule-based systems such as WAF are useful, but behavior-based ML detection can add another layer of protection.
- Cloud-native and AI technologies are converging. Managed services such as Lambda, API Gateway, DynamoDB, Bedrock, and Neptune help developers build advanced systems faster.
- Teamwork requires structure. Clear goals, good communication, and personal accountability are as important as technical ability.

## Connection to My Internship Project

This event was directly useful for my internship project on **AI Coding Agent Risk Scoring on AWS SageMaker**. The ML-based NIDS session helped me think about risk detection as a combination of model prediction and hard safety rules. The Docker and DevOps sessions reinforced the need for repeatable environments and careful deployment practices. The teamwork session helped me improve the way I document worklog progress and communicate project value.

Most importantly, the event strengthened my view that AI-assisted systems should be evaluated with evidence. Whether the system is detecting network attacks, generating answers through GraphRAG, or scoring AI coding-agent behavior, the engineering process must include logs, metrics, validation, and clear operational boundaries.

## Registration Evidence

| Registration Detail | Value |
|---|---|
| Full name | Chu Nguyễn Tuấn Anh |
| Email | anh.chunguyentuan@hcmut.edu.vn |
| Phone | 0962037357 |
| University | Trường Đại học Bách khoa |
| Student ID | 2352022 |
| Registration date | 06/06/2026 |
| Work shift | Fulltime |
| Floor | 26th Floor |
| Purpose | Attend Events |

## Participation Evidence

I forgot to take a personal photo during this event. The retained evidence is therefore the FCAJ Portal attendance-history record below, which shows my check-in for the 09:00 shift on 06/06/2026 at the 26th Floor. This screenshot documents portal attendance; it is not a personal event photo.

![FCAJ Portal attendance-history evidence for Event 1 on 06 June 2026](/images/events/event1-portal-checkin.png)

---

[Back to Events Participated](/4-eventparticipated/) | [Next](/4-eventparticipated/4.2-event2/)
