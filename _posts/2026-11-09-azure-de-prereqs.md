---
title: "What to know before starting an Azure data engineering project"
tags: [data-engineering, azure]
excerpt: "The prerequisite stack, programming, cloud basics, and core concepts, that's worth having in place before a project like an end-to-end Spotify Azure pipeline, not something to learn mid-build."
---

End-to-end Azure data engineering projects (Azure Data Factory, Databricks, ADLS Gen2, Unity Catalog, Delta Live Tables, and so on) tend to assume a baseline you're expected to already have. Here's what that baseline actually looks like, split from the tools you pick up while building the project itself.

## Core programming

- **Python**: functions, classes, file handling, and writing reusable, modular code. Projects like this lean on custom Python utilities and classes rather than one-off scripts.
- **SQL**: joins, window functions, aggregations, CTEs. You should be comfortable querying and manipulating relational data without having to look up syntax.

## Cloud fundamentals (Azure specifically)

- What a cloud platform actually is: resource groups, regions, access control (IAM/RBAC).
- Basic navigation of the Azure Portal.
- The core difference between IaaS and PaaS, since Azure Data Factory, Databricks and Azure SQL Database are all PaaS services you configure rather than servers you manage.

## Data engineering concepts (tool-agnostic)

- **ETL vs ELT**: whether you transform before or after loading.
- **Batch vs streaming ingestion**: the difference matters for how a pipeline is designed.
- **Medallion architecture** (Bronze / Silver / Gold): this kind of project assumes you already understand *why* that layering exists, not just that it does.
- **Incremental loading vs full loads**, and what backfilling means in that context.
- **Star schema basics**: fact tables, dimension tables.
- **Slowly Changing Dimensions (SCD)**, type 1 and type 2, conceptually, before seeing it implemented in a pipeline.

## Distributed processing basics

- What Spark is and why distributed processing exists. You don't need the internals yet, just the mental model: partitions, lazy evaluation, driver/executor.
- Basic PySpark syntax: DataFrames, the difference between transformations and actions.

## Version control

- Git fundamentals: commits, branches, merging.
- A GitHub account and a basic repo workflow, since CI/CD and Databricks Asset Bundles both assume this is already second nature, not something you're learning alongside them.

## Nice to have, not strictly required

- Basic YAML, since Databricks Asset Bundles configuration is YAML-based.
- Familiarity with Jinja templating syntax, used for parameterized, reusable pipeline code.

## Why this matters

A project like this, Azure SQL Database and GitHub as sources, Azure Data Factory for ingestion, ADLS Gen2 for storage, Databricks and PySpark for transformation, Unity Catalog for governance, Delta Live Tables for the gold layer, and Databricks Asset Bundles for deployment, is really a chain of tools stitched together around concepts that don't change. If the concepts (medallion architecture, SCD, incremental loading) are already solid, each new tool is just a new interface onto something familiar. If they're not, every new tool feels like starting from zero, because the real complexity was never in the tool to begin with.
