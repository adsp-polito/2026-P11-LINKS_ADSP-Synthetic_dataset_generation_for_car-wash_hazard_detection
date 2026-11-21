# Work breakdown structure

The project is split into work packages (WP), and each WP into tasks. Every task has one owner, one output and one acceptance check, so anyone can see what must be done, by whom and how we know it is finished.

The timeline for the same tasks is in the [Gantt chart](gantt.md).

## Owners

One task, one owner. Contributors can be many; the owner is accountable for the output.

| Owner | Area | Modules |
| --- | --- | --- |
| Leonardo Passeri | Design, management and human review | `docs/`, [interfaces](../../src/interfaces) |
| Role B | Data and models | [generation](../../src/generation), [labelling](../../src/labelling), `data/` |
| Role C | Validation and evaluation | [validation](../../src/validation), [export](../../src/export), [tests](../../tests) |

Role B and Role C are taken by Alessio D'Aiuto and Antonio Vetrone; the split between them is still to be confirmed. The person who builds a component does not evaluate it: Role B generates and segments, Role C measures whether it works, and the human review checks both.

## Milestones

| ID | Milestone | Date | Status |
| --- | --- | --- | --- |
| M1 | Checkpoint #1: project design | 11 Nov 2026 | Done |
| M2 | Checkpoint #2: first end-to-end pilot on six classes | 2 Dec 2026 | Planned |
| M3 | Checkpoint #3: integrated pipeline and labelled dataset | 18 Dec 2026, 21 Dec 2026 or 8 Jan 2027 (slot to be assigned) | Planned |
| M4 | Final submission: report, slides and notebook | One week before the exam (date to be announced) | Planned |

## Work packages

### WP1 · Design

Objective: define who the dataset is for, what it must contain and how we measure its quality.

| ID | Task | Owner | Output | Acceptance | Due | Status |
| --- | --- | --- | --- | --- | --- | --- |
| T1.1 | Users, personas and value proposition | Leonardo Passeri | Checkpoint #1 slides | Presented at M1 | 11 Nov | Done |
| T1.2 | Functional requirements R1-R3 | Leonardo Passeri | [tests README](../../tests/README.md) | Each requirement has an acceptance check | 11 Nov | Done |
| T1.3 | Pilot scope: six hazard classes | Leonardo Passeri | Class table in the [README](../../README.md) | Classes, allowed viewpoints and the remaining 17 classes are listed | 22 Nov | In progress |
| T1.4 | KPI cards | Leonardo Passeri | `docs/kpi_cards.md` | Each KPI has definition, baseline, target, method and owner, including one value KPI | 23 Nov | Planned |
| T1.5 | Annotation policy per class | Leonardo Passeri | `docs/annotation_policy.md` | For each class: what the mask covers, eligible cases and how to flag doubtful ones | 24 Nov | Planned |
| T1.6 | CompCars data assessment | Leonardo Passeri | Data assessment notebook | Viewpoint, body type and resolution distributions; known gaps towards car-wash cameras | 25 Nov | Planned |

### WP2 · Data and generation

Objective: add one hazard to each selected car photo, with every run reproducible.

| ID | Task | Owner | Output | Acceptance | Due | Status |
| --- | --- | --- | --- | --- | --- | --- |
| T2.1 | Environment on the GPU server and model access (FLUX.1 Kontext, SAM 3, CompCars) | Role B | Requirements file and pilot config | FLUX.1 Kontext edits one photo on the 16 GB server; time and memory are logged | 22 Nov | Planned |
| T2.2 | Shared metadata schema and folder layout | Role B | Schema in `src/` | All modules read and write the same fields | 22 Nov | Planned |
| T2.3 | Source photo selection | Role B | Manifest builder and `data/manifests/pilot_v1.csv` | Only viewpoints compatible with each class; fixed seed | 23 Nov | Planned |
| T2.4 | Hazard prompts | Role B | `data/prompts/` v1 | One versioned entry per pilot class | 23 Nov | Planned |
| T2.5 | Pilot generation | Role B | Edited images (outside git) and run log | About 250-300 edits with complete metadata; seconds per image measured | 26 Nov | Planned |
| T2.6 | Experiment E0 vs E1: prompt with and without a preservation clause | Role B | Experiment card in `results/` | Same photos and seeds; pass rate compared per class | 27 Nov | Planned |
| T2.7 | Full generation for the six classes | Role B | Generated dataset (outside git) | Target number of images per class reached | 9 Dec | Planned |

### WP3 · Validation

Objective: reject edits that miss the hazard, change the car or are physically implausible. Linked to R1.

| ID | Task | Owner | Output | Acceptance | Due | Status |
| --- | --- | --- | --- | --- | --- | --- |
| T3.1 | Test setup with small fixtures | Role C | pytest setup | Tests run on CPU without model weights | 23 Nov | Planned |
| T3.2 | Hazard presence check | Role C | Check in `src/validation` | Score per image: hazard found in the edit and not in the original | 25 Nov | Planned |
| T3.3 | Car preservation check | Role C | Check in `src/validation` | Score per image for changes outside the hazard region | 26 Nov | Planned |
| T3.4 | Plausibility check (first version) | Role C | Check in `src/validation` | Fixed checklist answered for each image | 27 Nov | Planned |
| T3.5 | Threshold calibration and agreement | Role C | Agreement report in `results/` | Thresholds set on split A; agreement, Cohen's kappa and false-accept rate measured on split B | 29 Nov | Planned |
| T3.6 | Validation v1 on the full dataset | Role C | Updated agreement report | Agreement measured on about 250 reviewed images | 12 Dec | Planned |

### WP4 · Labelling

Objective: one segmentation mask and class per accepted image. Linked to R2.

| ID | Task | Owner | Output | Acceptance | Due | Status |
| --- | --- | --- | --- | --- | --- | --- |
| T4.1 | SAM 3 labelling with text and box prompts | Role B | Labelling script | One non-empty mask per accepted image | 28 Nov | Planned |
| T4.2 | Hand-labelled reference masks | Leonardo Passeri (all three label) | Reference masks, drawn from scratch | About 100 masks for the pilot, about 140 for the final KPI | 29 Nov | Planned |
| T4.3 | Mask quality evaluation | Role C | IoU report in `results/` | Share of masks with IoU ≥ 0.5 and mean IoU, per class | 29 Nov | Planned |

### WP5 · Export

Objective: an annotated dataset that a detector can be trained on. Linked to R3.

| ID | Task | Owner | Output | Acceptance | Due | Status |
| --- | --- | --- | --- | --- | --- | --- |
| T5.1 | COCO export with train and validation split by car model | Role C | Export script | No car model appears in both splits | 30 Nov | Planned |
| T5.2 | Tests for R1, R2 and R3 | Role C | `tests/test_r1_generation.py`, `test_r2_labelling.py`, `test_r3_export.py` | Tests pass in CI | 30 Nov | Planned |
| T5.3 | Dataset release v1 and datasheet | Role C | Released dataset and its documentation | Passes integrity checks and loads in the target training pipeline | 16 Dec | Planned |
| T5.4 | Optional: one-epoch YOLO training test | Role C | Training log | The exported dataset trains without errors | 8 Jan | Optional |

### WP6 · Human review

Objective: measure whether the automatic checks and the masks can be trusted.

| ID | Task | Owner | Output | Acceptance | Due | Status |
| --- | --- | --- | --- | --- | --- | --- |
| T6.1 | Review app | Leonardo Passeri | App in [interfaces](../../src/interfaces) | Before and after side by side, pass or fail, failure type, mask check, time per image; automatic verdict hidden | 27 Nov | Planned |
| T6.2 | Review protocol and sampling | Leonardo Passeri | Protocol in `docs/` | Sample includes accepted and rejected images; 30 images reviewed by two people | 27 Nov | Planned |
| T6.3 | Pilot review sessions | Leonardo Passeri (all three review) | Decisions in `results/` | About 100 images per reviewer | 29 Nov | Planned |
| T6.4 | Final review sample | Leonardo Passeri (all three review) | Decisions in `results/` | About 250 images in total | 12 Dec | Planned |

### WP7 · Management and communication

Objective: keep the plan, the evidence and the presentations up to date.

| ID | Task | Owner | Output | Acceptance | Due | Status |
| --- | --- | --- | --- | --- | --- | --- |
| T7.1 | WBS, Gantt and tracking | Leonardo Passeri | This file and the [Gantt chart](gantt.md) | Updated at least once a week with plan versus actual | Weekly | In progress |
| T7.2 | Checkpoint #2 slides | Leonardo Passeri | `docs/presentations/checkpoint_2.pdf` | KPI review table, demo and experiment E0 vs E1 included | 1 Dec | Planned |
| T7.3 | Domain gap analysis | Leonardo Passeri | Section in the report | Differences between CompCars and car-wash cameras described and, where possible, measured | 14 Dec | Planned |
| T7.4 | Checkpoint #3 slides | Leonardo Passeri | `docs/presentations/checkpoint_3.pdf` | Technical and value KPIs against targets | 17 Dec | Planned |
| T7.5 | Final report (ACM template, 6 pages) | Leonardo Passeri | LaTeX report in `docs/` | All three requirements and KPIs reported with their evidence | M4 | Planned |
| T7.6 | Final notebook | Leonardo Passeri | Notebook in [notebooks](../../notebooks) | Runs the pipeline on a small sample end to end | M4 | Planned |

## Weekly tracking

Updated at every weekly review. A task is done when its acceptance check passes and the evidence is linked.

| Task | Owner | Planned finish | Actual or forecast finish | Evidence | Blocker | Next action |
| --- | --- | --- | --- | --- | --- | --- |
| T1.3 | Leonardo Passeri | 22 Nov | 22 Nov | Scope update pull request | None | Confirm the remaining 17 classes with LINKS |
| T2.1 | Role B | 22 Nov | To be confirmed | | Model access requests pending | Run the first FLUX.1 Kontext edit on the server |
| T7.1 | Leonardo Passeri | Weekly | 21 Nov | This file | None | Review on 28 Nov |
