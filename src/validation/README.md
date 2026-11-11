# validation

Rejects edits that are wrong or unrealistic before they are labelled.

It checks three things: the requested hazard is present, the rest of the car is unchanged, and the result is physically plausible.

| | |
| --- | --- |
| **Input** | The original and edited image, and the target class |
| **Output** | Accept or reject, with a score for each check. Rejected images are logged per class |
| **Owner** | To be assigned |
| **Check** | Tests linked to R1. KPI: at least 80% agreement with human review on a sample |
