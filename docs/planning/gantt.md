# Gantt chart

When each task in the [WBS](wbs.md) happens and what depends on what, up to Checkpoint #3. Task IDs match the WBS. Red bars are on the critical path to the next checkpoint.

```mermaid
gantt
    title ADSP P11 plan up to Checkpoint 3
    dateFormat YYYY-MM-DD
    axisFormat %d %b

    section Milestones
    M1 Checkpoint 1                     :milestone, done, m1, 2026-11-11, 0d
    M2 Checkpoint 2                     :milestone, crit, m2, 2026-12-02, 0d
    M3 Checkpoint 3 first slot          :milestone, m3a, 2026-12-18, 0d
    M3 Checkpoint 3 second slot         :milestone, m3b, 2026-12-21, 0d
    M3 Checkpoint 3 third slot          :milestone, m3c, 2027-01-08, 0d

    section WP1 Design
    T1.1 Users and value proposition    :done, t11, 2026-10-28, 14d
    T1.2 Requirements R1-R3             :done, t12, 2026-11-04, 7d
    T1.3 Pilot scope six classes        :active, t13, 2026-11-21, 2d
    T1.4 KPI cards                      :t14, 2026-11-22, 2d
    T1.5 Annotation policy              :t15, 2026-11-23, 2d
    T1.6 CompCars data assessment       :t16, 2026-11-24, 2d

    section WP2 Data and generation
    T2.1 Server setup and model access  :crit, t21, 2026-11-21, 2d
    T2.2 Metadata schema                :t22, 2026-11-21, 2d
    T2.3 Source photo selection         :t23, 2026-11-22, 2d
    T2.4 Hazard prompts                 :t24, 2026-11-22, 2d
    T2.5 Pilot generation               :crit, t25, 2026-11-24, 3d
    T2.6 Experiment E0 vs E1            :t26, 2026-11-26, 2d
    T2.7 Full generation six classes    :t27, 2026-12-03, 7d

    section WP3 Validation
    T3.1 Test setup                     :t31, 2026-11-22, 2d
    T3.2 Hazard presence check          :t32, 2026-11-23, 3d
    T3.3 Car preservation check         :t33, 2026-11-24, 3d
    T3.4 Plausibility check v0          :t34, 2026-11-26, 2d
    T3.5 Calibration and agreement      :crit, t35, 2026-11-27, 3d
    T3.6 Validation v1 full dataset     :t36, 2026-12-08, 5d

    section WP4 Labelling
    T4.1 SAM 3 labelling                :crit, t41, 2026-11-26, 3d
    T4.2 Hand-labelled masks            :t42, 2026-11-27, 3d
    T4.3 Mask IoU evaluation            :t43, 2026-11-29, 1d

    section WP5 Export
    T5.2 Tests R1 R2 R3                 :t52, 2026-11-28, 3d
    T5.1 COCO export                    :t51, 2026-11-29, 2d
    T5.3 Dataset release v1             :t53, 2026-12-12, 5d
    T5.4 Optional YOLO training test    :t54, 2026-12-21, 18d

    section WP6 Human review
    T6.1 Review app                     :t61, 2026-11-25, 3d
    T6.2 Review protocol and sampling   :t62, 2026-11-26, 2d
    T6.3 Pilot review sessions          :crit, t63, 2026-11-28, 2d
    T6.4 Final review sample            :t64, 2026-12-07, 6d

    section WP7 Management and communication
    T7.1 Weekly tracking                :active, t71, 2026-11-21, 2027-01-08
    T7.2 Checkpoint 2 slides            :crit, t72, 2026-11-29, 3d
    T7.3 Domain gap analysis            :t73, 2026-12-08, 7d
    T7.4 Checkpoint 3 slides            :t74, 2026-12-14, 4d
    T7.5 Final report                   :t75, 2026-12-10, 30d
    T7.6 Final notebook                 :t76, 2026-12-21, 18d
```

## Dependencies

The critical path to Checkpoint #2 runs through the GPU server: nothing can be measured until the first edits exist.

| Task | Needs | Why |
| --- | --- | --- |
| T2.5 Pilot generation | T2.1, T2.3, T2.4 | FLUX.1 Kontext must run on the server, with the photos and prompts chosen |
| T3.5 Calibration and agreement | T2.5, T3.2-T3.4, T6.3 | Thresholds are set and checked against human decisions on generated images |
| T4.1 SAM 3 labelling | T2.5 | Masks are drawn on the generated images |
| T4.3 Mask IoU evaluation | T4.1, T4.2, T1.5 | IoU needs SAM 3 masks, reference masks and one shared definition of each mask |
| T6.3 Pilot review sessions | T2.5, T6.1, T6.2 | Reviewers need images, the app and the sampling protocol |
| T5.1 COCO export | T2.2, T4.1 | The export reads the shared metadata and the masks |
| T7.2 Checkpoint #2 slides | T2.6, T3.5, T4.3 | The slides report the experiment and the first value of each KPI |

T3.2-T3.4, T6.1 and T5.2 do not wait for the server: they start on small placeholder images that follow the shared metadata schema (T2.2).

## Schedule risks

| Risk | Affected tasks | Response |
| --- | --- | --- |
| SAM 3 or CompCars access arrives late | T2.3, T4.1 | Request access on day one; start with the photos already downloaded |
| FLUX.1 Kontext is slow on the 16 GB server | T2.5, T2.7 | Measure seconds per image in T2.1 and size the pilot to fit; run overnight |
| The server is shared with other users | T2.5, T2.7 | Book GPU time and run the stages one after the other |
| Fewer than 50% of a class's edits pass validation | T2.7 | Revise the prompt once; if it still fails, drop the class as the KPI rule says |
