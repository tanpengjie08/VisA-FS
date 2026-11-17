# VisA-FS

**VisA-FS** is a fine-grained few-shot benchmark for joint industrial **anomaly localization and classification**, built by re-categorizing and re-annotating [VisA](https://github.com/amazon-science/visual-anomaly).

> **The dataset will be released soon.** Images, corrected instance masks, fine-grained defect labels, and the official train/test splits will be published in this repository.

## Why VisA-FS

Public anomaly datasets mostly support either pixel-level localization or coarse anomaly detection. They are a poor fit for few-shot defect classification: many defect types have too few samples, masks contain omissions, and labels are not instance-level.

VisA-FS closes that gap for the unified localization-and-classification setting used by [UniALC](https://github.com/tanpengjie08).

## What changed from VisA

| | VisA | VisA-FS |
|---|---|---|
| Defective images | 1,200 | subset after filtering rare types |
| Product categories | 12 | 11 (`PCB4` removed) |
| Defect types | 74 (1–69 images each) | **50** |
| Masks | original pixel masks | manually corrected; missed defects filled in |
| Split | detection-oriented | ~40% train / ~60% test **per defect type** |

Construction rules:

1. Drop defect types with fewer than 8 instances.
2. Drop `PCB4`, which would have been left with a single defect type.
3. Re-annotate every defect region: fix wrong masks and add defects the original labels missed.
4. Assign a fine-grained defect-type label to each instance.
5. For each remaining defect type, put about 40% of the images in the support/train pool and about 60% in the query/test pool.

## Evaluation protocol

Standard **N-way K-shot** classification on the held-out query set.

- Support images are sampled from the train split; the query set is the full test split.
- Because some types are still small, report **K ∈ {1, 2}**.
- Average accuracy over **4** independent support-set draws.
- Localization backbones produce an anomaly heatmap; a single-image binarizer (BinSPAA in UniALC) turns it into a mask before classification.

Reference numbers from UniALC (Dinomaly2 heatmap + BinSPAA), mean accuracy over 4 runs:

| Setting | UniALC |
|---|---|
| 1-shot | 56.1% |
| 2-shot | 63.4% |

## Planned layout

```
VisA-FS/
├── <category>/
│   ├── train/
│   │   ├── <defect_type>/
│   │   │   ├── images/
│   │   │   └── masks/
│   └── test/
│       └── <defect_type>/
│           ├── images/
│           └── masks/
└── meta.json          # category, defect type, split, image id
```

`meta.json` and the exact directory names will ship with the first release. Do not assume filenames from upstream VisA until then.

## Coming with the release

- Corrected images and instance masks
- Defect-type labels and the fixed train/test split
- A short loading example
- License and attribution notes (VisA-FS is derived from VisA)

## Citation

If you use VisA-FS, please cite both this benchmark and the original VisA dataset. A BibTeX entry will be added when the paper and the data are released.

## Contact

Questions and issue reports: [github.com/tanpengjie08/VisA-FS](https://github.com/tanpengjie08/VisA-FS).
