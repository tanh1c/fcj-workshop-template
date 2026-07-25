---
title: "Week 1: Onboarding and AWS Foundation"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

## 01/06/2026 - 07/06/2026

**Work mode:** Individual implementation combined with group-based learning and discussion.
**Program:** Workforce Bootcamp - First Cloud AI Journey.
**Mentor:** No fixed mentor assigned; work was self-managed and supported by documentation, tutorials, and peer discussion.

## Objective

Understand the internship requirements, establish a working plan, and build the AWS foundation needed before selecting and implementing the technical project.

## Context

The first week was mainly used to move from a general bootcamp learning context into a concrete internship direction. Since the program did not assign a fixed mentor, the working style had to be self-managed: define weekly goals, collect evidence, and keep the final report requirements in mind from the beginning.

## AWS Learning Focus

Before choosing the project implementation path, I spent this week building a foundation in core AWS concepts. The goal was to understand what each AWS service is responsible for before mapping it into the final MVP.

- **AWS Global Infrastructure:** Studied Regions and Availability Zones to understand why the project should consistently use one Region, especially `ap-southeast-1`, for S3, SageMaker, Lambda, and API Gateway resources.
- **AWS Identity and Access Management (IAM):** Reviewed users, roles, policies, trust relationships, and the least-privilege principle. This was important because the project would later need separate permissions for SageMaker and Lambda.
- **Shared Responsibility Model:** Learned which responsibilities belong to AWS and which belong to the user, especially around credentials, IAM permissions, data access, and cleanup of created resources.
- **Amazon S3 concept:** Studied buckets, objects, prefixes, and storage use cases so project data could later be organized into raw, processed, and model artifact locations.
- **Amazon CloudWatch overview:** Reviewed CloudWatch as the service used for logs and operational visibility when managed AWS jobs or functions run.
- **Initial AWS service mapping:** Connected the project idea to a simple service chain: S3 for storage, SageMaker for ML workflow, Lambda/API Gateway for API exposure, CloudWatch for logs, and IAM for access control.

After this learning step, I had enough AWS context to choose a project scope that was realistic for a student AWS account and suitable for an internship report.

## Daily Breakdown

| Date | Work performed |
|---|---|
| 01/06/2026 | Reviewed internship requirements and identified all required report sections. |
| 02/06/2026 | Studied AWS account basics, global infrastructure, Regions, and Availability Zones. |
| 03/06/2026 | Reviewed IAM, shared responsibility, and security responsibilities for student AWS projects. |
| 04/06/2026 | Compared possible project topics and selected AI Coding Agent risk scoring as the main direction. |
| 05/06/2026 | Drafted the initial MVP service map using S3, SageMaker, Lambda, API Gateway, CloudWatch, and IAM. |
| 06/06/2026 - 07/06/2026 | Organized notes, references, and initial evidence for the Worklog and final report. |


## Technical Activities

- Reviewed the required report structure and identified that the final output must include worklog, proposal, blogs, events, workshop, self-evaluation, and feedback.
- Studied AWS fundamentals such as Regions, Availability Zones, IAM, shared responsibility, and the difference between storage, compute, networking, and managed ML services.
- Compared project directions and selected AI Coding Agent risk scoring because it combines AI safety, software engineering evidence, and AWS ML deployment.
- Outlined the expected AWS service chain: S3 for data, SageMaker for processing/model hosting, Lambda and API Gateway for API exposure, CloudWatch for logs, and IAM for permissions.

## Deliverables

- **Internship topic selected.**
- **Initial report outline created.**
- **AWS learning scope defined.**
- **High-level MVP service map drafted.**

## Challenge and Solution

**Challenge:** The main challenge was narrowing a broad AI/cloud learning scope into one project that was realistic for a student AWS account.

**Solution:** The scope was limited to an honest MVP: score agent runs from trajectory logs, demonstrate the AWS workflow, and clearly separate implemented parts from future MLOps extensions.

## Project Relevance

This week contributed to the final MVP by strengthening the path from **AI coding-agent behavior evidence** to an **AWS-based risk scoring workflow**. The work helped ensure that the final workshop is not only a conceptual explanation, but also follows the actual implementation sequence used in the project.

## Evidence Screenshots

![AWS Cloud Practitioner learning page](/images/worklog/week01-aws-learning.png)

The screenshot shows the AWS Cloud Practitioner learning page used as the starting point for AWS foundation study.

## Evidence and References Studied

- [AWS Cloud Practitioner Essentials](https://aws.amazon.com/training/digital/aws-cloud-practitioner-essentials/)
- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)
- [IAM security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

---

[Back to Worklog](/1-worklog/) | [Next](/1-worklog/1.2-week2/)
