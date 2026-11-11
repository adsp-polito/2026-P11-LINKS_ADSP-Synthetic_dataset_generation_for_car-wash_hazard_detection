# interfaces

The tool people use to review a sample of the pipeline's decisions.

A reviewer sees each image before and after the edit, marks it as pass or fail, and picks a failure type. The sample includes rejected images too, so wrong rejections are also caught.

| | |
| --- | --- |
| **Input** | A sample of accepted and rejected images |
| **Output** | Reviewer decisions, compared with the automatic checks |
| **Owner** | To be assigned |
| **Check** | Supports the KPI: at least 80% agreement between automatic checks and human review |
