---
title: "When a data pipeline has too many ‘final’ versions"
date: 2026-07-17
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

## AWS Architecture Blog | When a data pipeline has too many “final” versions

| Information | Details |
|---|---|
| Publication date | 17/07/2026 |
| Status | Published |
| Platform | AWS Study Group - Facebook Group |
| Published post | [View the post on Facebook](https://www.facebook.com/groups/awsstudygroupfcj/?multi_permalinks=2216239979140962) |
| AWS article studied | *Specification-driven composition for flexible data workflows* — AWS Architecture Blog |

Hello everyone,

Many of us have probably seen a project containing files such as `clean_data.py`, `clean_data_v2.py`, and `clean_data_final.py`, until nobody remembers which file is actually current.

I recently read **“Specification-driven composition for flexible data workflows”** on the AWS Architecture Blog. The article presents an interesting approach: instead of writing a separate pipeline for every dataset, we describe the required steps in a JSON or YAML specification and let the system compose the corresponding workflow.

For example, a configuration file may only need to describe:

```text
order_date → format_date
amount → normalize_currency
email → validate_email
```

Functions such as date formatting and email validation are implemented and tested once, then reused across multiple pipelines.

![Specification-Driven Composition design pattern](/images/blogs/blog1-specification-driven-design-pattern.webp)

*Figure 1. Specification-Driven Composition design pattern.*

The architecture can be understood as the following flow:

```text
Specification uploaded to Amazon S3
↓
AWS Lambda validates the configuration
↓
Capability discovered in Amazon OpenSearch Service
↓
AWS Step Functions creates and orchestrates the workflow
↓
AWS Lambda functions process the data
```

![AWS implementation of Specification-Driven Composition](/images/blogs/blog1-aws-implementation.webp)

*Figure 2. AWS implementation of Specification-Driven Composition.*

The first Lambda function acts like an assembler. It verifies that the specification is valid, that the requested processing capabilities exist, and that each step’s input is compatible with the previous step’s output.

AWS Step Functions then orchestrates the steps and tracks workflow state. If a step fails, the system can retry it or move to an error-handling path instead of stopping the entire pipeline without a clear explanation.

I think this approach is suitable for systems that process many data sources with similar structures.

For example, a company may receive order files from many partners. Each partner uses different column names, date formats, and currencies. Instead of copying an old script and modifying it for every partner, the data team can maintain a shared set of capabilities and create only a new specification.

However, moving logic from Python to JSON does not automatically make the system simple.

The organization still needs to build the Composer, version its capabilities, validate schemas, and control who may modify specifications. Without careful governance, the team may only move the disorder from Python files into JSON files.

Security also matters. A specification should only be allowed to use capabilities that have been registered and approved. If users can insert an arbitrary Lambda ARN or arbitrary code, the platform may become an uncontrolled code-execution system.

In my view, this pattern is unnecessary for a small project with only a few pipelines. A clear script or an explicit Step Functions workflow may be easier to maintain.

It becomes worth considering when many pipelines duplicate the same logic, new datasets are introduced frequently, and one rule change requires updates across many scripts.

My main takeaway is that complexity does not disappear. It moves from individual pipelines into a shared platform. When the number of workflows is large enough, this can be a reasonable trade-off; applying it too early may become over-engineering.

When do you think a project should start building a shared platform: when it has many datasets, or when copy-pasted pipeline code becomes common?

Thank you for reading!

## References

- [Specification-driven composition for flexible data workflows](https://aws.amazon.com/blogs/architecture/specification-driven-composition-for-flexible-data-workflows/) — AWS Architecture Blog.
- [AWS Step Functions Developer Guide](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html)

---

[Back to Blogs Posted](/3-blogsposted/) | [Next](/3-blogsposted/3.2-blog2/)
