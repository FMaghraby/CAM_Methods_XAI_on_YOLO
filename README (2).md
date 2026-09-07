# Layer-Wise Evaluation of CAM Techniques in YOLOv8

Reproducibility materials for the study:

> **Assessing the Impact of Layer Selection on CAM-Based Explainability for YOLOv8: A Study on Hand-Sketched Digital Logic Circuits**

This repository contains the Python notebook used to generate and evaluate class activation maps (CAMs) for a YOLOv8s object detector trained on hand-sketched digital logic circuits.

## Authors

- Noha A. ElMasry
- Fahima A. Maghraby
- Mohamed Waleed Fakhr

## Dataset

The dataset is maintained separately and is not duplicated in this code repository.

- **Zenodo:** https://doi.org/10.5281/zenodo.22553253
- **GitHub:** https://github.com/FMaghraby/Hand-Sketched-DLCs
- **Dataset license:** Creative Commons Attribution 4.0 International (CC BY 4.0)

Download and extract the dataset before running the notebook. Preserve its `train`, `valid`, and `test` partitions and update the dataset path in the notebook or configuration file.

## Included notebook

```text
CAM_Methods_XAI_on_YOLO_.ipynb
```

The notebook demonstrates environment preparation, dataset access, CAM generation, metric computation, result aggregation, plotting, and export.

## CAM methods

The study evaluates:

- Grad-CAM
- Grad-CAM++
- HiResCAM
- XGradCAM
- LayerCAM
- EigenCAM
- EigenGradCAM

## Evaluated layer configurations

The manuscript reports the following individual and fused YOLOv8 layer configurations:

```text
[2]
[4]
[6]
[9]
[18]
[21]
[3, 4, 5]
[6, 7, 8]
[9, 12, 15]
[18, 21]
```

Ensure that the notebook loops over these configurations separately when reproducing the complete layer-wise analysis.

## Evaluation metrics

The generated CAMs are evaluated using:

- **Focus Score:** fraction of saliency energy located within the target ground-truth region.
- **Pointing Game hit rate (PG-Hit):** whether the maximum-saliency point lies inside the target ground-truth box.
- **Intersection over Union (IoU):** overlap between a thresholded CAM mask and the target ground-truth region.
- **Sharpness:** variance of the Laplacian of the normalized CAM.

For reproducible evaluation, compute these metrics from the raw CAM array rather than the colored visualization overlay. Use ground-truth YOLO annotations and keep the CAM threshold, normalization, box-conversion rule, and aggregation procedure fixed across all methods.

## Requirements

The original experimental environment used:

- Python 3
- PyTorch 2.0
- CUDA 11.8 for detector training
- Ultralytics `8.0.210`
- YOLOv8-Explainer
- NumPy
- OpenCV
- pandas
- Pillow
- Matplotlib
- FPDF
- Roboflow client, if downloading from Roboflow

Install dependencies in a clean virtual environment. A fully pinned `requirements.txt` or `environment.yml` should be included with the archived release.

## Model and data paths

Before running the notebook, replace its placeholder values with local paths or environment variables:

```python
img_folder = "/path/to/dataset/valid/images"
model_path = "/path/to/best.pt"
output_base = "/path/to/results"
```

Do not commit Roboflow API keys, access tokens, passwords, personal Google Drive paths, or other credentials. Use an environment variable if authenticated access is required:

```python
import os
roboflow_api_key = os.environ.get("ROBOFLOW_API_KEY")
```

## Recommended execution sequence

1. Download and extract the dataset.
2. Create and activate a clean Python environment.
3. Install the pinned dependencies.
4. Place the trained YOLOv8s checkpoint in a local `weights/` directory or download it from the associated Zenodo software record.
5. Set the dataset, checkpoint, and output paths.
6. Run the notebook from top to bottom.
7. Verify that all seven CAM methods and all ten layer configurations completed successfully.
8. Retain per-image metrics before calculating summary means.

## Expected outputs

The reproducibility release should preserve:

```text
results/
├── per_image_metrics.csv
├── summary_metrics.csv
├── latency_results.csv
└── cam_visualizations/
```

Each result row should record the image identifier, CAM method, target-layer configuration, target class or detection, metric values, checkpoint identifier, and relevant thresholds.

## Reproducibility notes

- Use one fixed detector checkpoint for all CAM methods and layer configurations.
- Do not use the final held-out test set for threshold or layer selection.
- Record the exact random seed, detector initialization, augmentation settings, confidence threshold, non-maximum-suppression threshold, CAM normalization, and IoU-mask threshold.
- Archive the trained checkpoint with a SHA-256 checksum.
- The reported detector was trained once; the published results do not quantify variability across independent training seeds.

## Software archive

The versioned software release will be archived on Zenodo. Replace the placeholder below after Zenodo assigns the software DOI:

```text
Software DOI: [ADD SOFTWARE DOI]
```

The dataset and software are separate research outputs. The software Zenodo record should list dataset DOI `10.5281/zenodo.22553253` as a related work with the relationship **Requires**.

## Citation

Until the software DOI is assigned, cite the associated manuscript and dataset. After publication, replace the placeholder with the complete software citation:

```text
ElMasry, N. A., Maghraby, F. A., and Fakhr, M. W. (2026).
Reproducibility Code for Layer-Wise Evaluation of CAM Techniques in YOLOv8
(Version 1.0.0) [Computer software]. Zenodo. [10.5281/zenodo.22649426]
```

Dataset citation:

```text
ElMasry, N. A., Maghraby, F. A., and Fakhr, M. W. (2026).
DLC-OD: A Hand-Sketched Digital Logic Circuits Dataset for Object Detection
(Version 1.0.0) [Data set]. Zenodo.
https://doi.org/10.5281/zenodo.22553253
```

## License

Copyright © 2026 Noha A. ElMasry, Fahima A. Maghraby, and Mohamed Waleed Fakhr.

The original code in this repository is released under the MIT License. See [`LICENSE`](LICENSE) for details. The DLC-OD dataset is licensed separately under CC BY 4.0. Third-party libraries and tools remain subject to their respective licenses.

## Contact

For questions about the code or experimental protocol, open an issue in this repository.
