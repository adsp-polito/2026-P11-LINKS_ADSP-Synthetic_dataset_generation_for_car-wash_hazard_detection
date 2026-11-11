# tests

Each test is linked to a functional requirement, so a passing test is evidence that the requirement is met.

| ID | Requirement | Acceptance |
| --- | --- | --- |
| R1 | **Generate hazard scenarios.** The system generates synthetic images of hazards such as open doors, hoods and trunks | Generated images are reviewed to verify that the requested hazards are clearly represented |
| R2 | **Generate labelled annotations.** The system produces segmentation masks and labels for the safety-critical objects in each image | A reviewer verifies that masks and labels match the corresponding objects |
| R3 | **Validate and export the dataset.** The system checks annotation consistency and exports the images and labels | The dataset passes integrity checks and loads in the target training pipeline |

Name test files after the requirement they cover, for example `test_r1_generation.py`.
