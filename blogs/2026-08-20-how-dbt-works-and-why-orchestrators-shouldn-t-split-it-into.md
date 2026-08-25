---
title: "How dbt works, and why orchestrators shouldn't split it into tasks"
url: "https://www.windmill.dev/blog/how-dbt-works-and-its-orchestrators"
date: "2026-08-20"
author: "Ruben Fiszel"
feed_url: "https://www.windmill.dev/blog/atom.xml"
---
How is dbt actually orchestrated, and why does running it as one job beat one task per model? A primer on dbt as a compiler with a scheduler attached, the five ways teams wrap it, and why both Dagster and astronomer-cosmos converged on a single dbt invocation with per-model state projected out of it.
