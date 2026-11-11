# labelling

Adds one segmentation mask and class to each accepted image with [SAM 3](https://github.com/facebookresearch/sam3).

| | |
| --- | --- |
| **Input** | An accepted image and its hazard class |
| **Output** | A segmentation mask and class for the hazard |
| **Owner** | To be assigned |
| **Check** | Tests linked to R2. KPI: at least 90% of masks reach IoU 0.5 against a hand-labelled sample |
