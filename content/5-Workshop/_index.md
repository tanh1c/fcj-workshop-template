---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

## AI Coding Agent Risk Scoring on AWS

This workshop explains the completed data, managed ML, governance, historical serving, and monitoring workflow for scoring AI Coding Agent trajectories. It uses accepted AWS evidence and safe placeholder commands; no paid resource needs to be rerun merely to follow the workshop.

## Workshop Sections

1. [Workshop Overview](/5-workshop/5.1-workshop-overview/)
2. [Prerequisites and Safety Gate](/5-workshop/5.2-prerequiste/)
3. [Split-Region Architecture](/5-workshop/5.3-s3-vpc/)
4. [Prepare Trajectory Dataset](/5-workshop/5.4-s3-onprem/)
5. [Run SageMaker Processing](/5-workshop/5.5-policy/)
6. [Managed Training, Evaluation, and HPO](/5-workshop/5.6-cleanup/)
7. [Pipeline and Model Registry Governance](/5-workshop/5.7-deploy-endpoint/)
8. [Historical Endpoint and Scoring API](/5-workshop/5.8-scoring-api/)
9. [End-to-End Validation and Evidence](/5-workshop/5.9-end-to-end-demo/)
10. [Monitoring and Cost Control](/5-workshop/5.10-monitoring-cost-control/)
11. [Cleanup](/5-workshop/5.11-cleanup/)

The managed Registry packages in `us-east-1` and the earlier locally trained artifact used by historical serving in `ap-southeast-1` are separate evidence tracks. Passing the model-quality gate enables registration only; it does not approve or deploy a model.
