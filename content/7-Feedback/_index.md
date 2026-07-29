---
title: "Sharing and Feedback"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

## Sharing and Feedback

This section summarizes my personal reflection and feedback after participating in the **Workforce Bootcamp - First Cloud AI Journey** program at **Amazon Web Services Viet Nam Company Limited**. The comments below are based on my experience studying AWS fundamentals, joining community events, and building the project **End-to-End Risk Scoring and Quality Evaluation System for AI Coding Agents on AWS SageMaker**.

## Overall Evaluation

**1. Learning environment**  
The program created an open and self-directed learning environment. I had space to explore AWS services independently, test commands, read documentation, and build a project from idea to MVP. This style was helpful because it encouraged me to take ownership instead of only following fixed instructions.

**2. Support from the program and community**  
Although there was no fixed mentor for my individual project, I still learned through group discussion, AWS documentation, workshops, and FCAJ community events. The events provided useful technical and career perspectives, especially around cloud engineering, DevOps, security, data analytics, and generative AI use cases.

**3. Relevance to my academic major**  
The internship was relevant to my Computer Science background because it connected programming, data processing, machine learning, API design, and system deployment. University knowledge helped me understand the model and data workflow, while the program helped me apply that knowledge in a cloud environment with AWS services.

**4. Learning and skill development opportunities**  
I improved both technical and professional skills during the program. Technically, I practiced Amazon S3, SageMaker Processing, managed XGBoost Training and Evaluation, Experiments/HPO, Pipeline, Model Registry, historical Endpoint serving, Lambda, API Gateway, Data Capture, Model Monitor, IAM, and CloudWatch. Professionally, I practiced governance design, cost-aware cleanup, evidence collection, honest evaluation, and bilingual workshop writing.

**5. Program culture and team spirit**  
The program encouraged learning through sharing and community participation. The technical events made the internship feel broader than an individual project, because I could listen to different speakers and understand how AWS skills are applied in real companies, career paths, and production systems.

**6. Internship policies and project scope**  
The self-learning format was suitable for exploring a personal technical topic. The unavailable Training quota in `ap-southeast-1` first required a bounded historical-serving path; approval in `us-east-1` later enabled the managed ML and governance workflow. This taught me to adapt Region and evidence strategy without conflating a temporary workaround with the final architecture.

## Most Satisfying Experience

The most satisfying part of the internship was completing a governed end-to-end workflow instead of only writing a proposal. Starting from AI coding-agent trajectories, I completed managed Processing, Training, held-out Evaluation, Experiments/HPO, conditional Registry registration, historical serving, Data Capture, Model Monitor, CloudWatch acceptance, and verified cleanup of paid resources.

Another valuable part was testing the perfect synthetic result rather than defending it. The frozen model's macro F1 fell from `1.00` internally to `0.1212` on the 40-sample External/OOD pilot. That result made the report stronger by showing why manual approval, human review, hard safety rules, and representative human-labeled data remain necessary.

## Suggestions for Improvement

- Provide an early checklist about AWS student-account quotas, especially for SageMaker Training and endpoint-related resources.
- Provide a cost-control checklist before learners deploy resources that may continue charging, such as SageMaker Endpoints.
- Share more example architectures for beginner-friendly ML/MLOps projects using S3, SageMaker, Lambda, API Gateway, IAM, and CloudWatch.
- Add optional milestone reviews so participants can receive feedback before the final report stage.
- Provide more guidance on what evidence should be captured before cleaning up AWS resources.

## Recommendation

I would recommend the First Cloud AI Journey program to friends who want to learn cloud computing through practical work. It is especially useful for students who already know programming or machine learning basics and want to understand how those skills fit into a real AWS workflow.

The program requires self-discipline because many tasks are self-managed, but that is also one of its strengths. It helps participants build independence, documentation habits, and a more realistic engineering mindset.

## Future Expectations

If I continue developing this project, I would first collect representative trajectories with independent human labels and add adapters for more agent frameworks. Only then would I evaluate calibrated thresholds or cost-sensitive learning, pin compatible runtimes, and design canary/rollback controls plus a separate reviewed deployment pipeline for a model that has received manual Registry approval.
