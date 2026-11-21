# tests

Each test is linked to a functional requirement, so a passing test is evidence that the requirement is met.

**Owner:** Role C, validation and evaluation (Alessio D'Aiuto or Antonio Vetrone, to be confirmed)

| ID | Requirement | Acceptance |
| --- | --- | --- |
| R1 | **Generate hazard scenarios.** The system generates synthetic images of the hazard classes listed in the [README](../README.md#hazard-classes), one hazard per image. The pilot covers open front door, open trunk or tailgate, open hood, roof box, roof bike rack and exposed towball | A sample of generated images is reviewed to verify that the requested hazard is clearly represented and the rest of the car is unchanged |
| R2 | **Generate labelled annotations.** The system produces segmentation masks and labels for the safety-critical objects in each image | A reviewer verifies that masks and labels match the corresponding objects |
| R3 | **Validate and export the dataset.** The system checks annotation consistency and exports the images and labels | The dataset passes integrity checks and loads in the target training pipeline |

R1 now names the hazard class list. The Checkpoint #1 slides also mentioned people near vehicles, which is not one of the hazard classes and is out of scope.

Name test files after the requirement they cover, for example `test_r1_generation.py`.
