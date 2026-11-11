# Synthetic dataset generation for car-wash hazard detection

Open doors, roof boxes and bike racks can jam or damage an automatic car wash. A camera at the entrance could spot them, but there is no real footage to train it on.

We are building an automatic pipeline that adds hazards to car photos, rejects unrealistic results and labels the rest. The output is a dataset that a hazard detector can later be trained on.

> **Status: design phase.** Checkpoint #1 was presented on 11 November 2026 ([slides](docs/presentations/checkpoint_1.pdf)). There is no pipeline code yet.

*Applied Data Science Project, Politecnico di Torino, with LINKS Foundation.*

## How it works

Each step is a module in `src/`, with the same name.

```mermaid
flowchart TD
    A[Car photos from CompCars] --> B[generation<br/>add one hazard with FLUX.1 Kontext]
    P[Hazard prompts] --> B
    B --> C{validation<br/>do the automatic checks pass?}
    C -- No --> D[Discard and log per class]
    C -- Yes --> E[labelling<br/>one mask and class with SAM 3]
    E --> F[export<br/>annotated synthetic dataset]
    D -. sample .-> H[interfaces<br/>human review]
    F -. sample .-> H
```

The pipeline runs without manual steps. People only review a sample, to measure whether the automatic checks can be trusted.

## What success looks like

| Question | Target |
| --- | --- |
| Can the model draw this hazard? | At least 50% of a class's images pass validation. Classes below this are dropped. |
| Can we trust the automatic checks? | At least 80% agreement with human review on a sample. |
| Are the labels usable? | At least 90% of masks reach IoU 0.5 against a hand-labelled sample. |

## Repository structure

```text
README.md          what the project is and how to run it
data/              prompts and manifests (no images, see licences below)
src/
  generation/      adds one hazard to a car photo
  validation/      rejects wrong or unrealistic edits
  labelling/       one mask and class per accepted image
  export/          checks and writes the annotated dataset
  interfaces/      human review of a sample
tests/             tests linked to requirements R1-R3
notebooks/         pilot experiments and the final notebook
results/           quality reports: pass rates, agreement, IoU
docs/              slides and reports
```

| Module | Input | Output | Check |
| --- | --- | --- | --- |
| [generation](src/generation) | Car photo and hazard prompt | Edited image and metadata | R1 |
| [validation](src/validation) | Original and edited image, target class | Accept or reject, with check scores | R1 |
| [labelling](src/labelling) | Accepted image and class | Segmentation mask and class | R2 |
| [export](src/export) | Images, masks and metadata | Annotated synthetic dataset | R3 |
| [interfaces](src/interfaces) | Sample of accepted and rejected images | Reviewer decisions | Agreement KPI |

Each module's README names its owner. The requirements are listed in [tests](tests).

## Hazard classes

The project covers 23 classes. These 16 are named in the project brief, and we are confirming the rest.

| Group | Classes |
| --- | --- |
| Openings | Doors, trunk, hood, fuel flap, windows, sunroof |
| Roof | Roof box, rails, crossbars, bike rack |
| Loads | Ladder, kayak, surfboard |
| Rear | Towbar rack, exposed towball, trailer |

## Data and models

| Step | Resource | Note |
| --- | --- | --- |
| Source photos | [CompCars](https://mmlab.ie.cuhk.edu.hk/datasets/comp_cars/) | Non-commercial research only |
| Image editing | [FLUX.1 Kontext [dev]](https://bfl.ai/blog/flux-1-kontext-dev) | Non-commercial licence |
| Labelling | [SAM 3](https://github.com/facebookresearch/sam3) | Checkpoints require access approval |

Images are not stored in this repository. We are checking what the licences allow us to publish.

## Scope

- **In scope:** the full pipeline on CompCars, and the labelled dataset.
- **Out of scope:** training the final hazard detector. It is a possible extension if time allows.

## Documents

- [Checkpoint #1 slides](docs/presentations/checkpoint_1.pdf), 11 November 2026
- [Project pitch](LINKS_ADSP-Project_Synthetic_dataset_generation_for_car-wash_hazard_detection.pdf), from LINKS Foundation

## Getting started

Setup instructions will arrive with the first code.

## Team

Leonardo Passeri, Alessio D'Aiuto, Antonio Vetrone
