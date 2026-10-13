# ⚡ VisiMac Studio

> **High-Performance Computer Vision & Dataset Preparation Engine**

**VisiMac Studio** is an all-in-one workstation designed for computer vision engineers, AI researchers, and data scientists. It bridges the gap between raw data collection and production model deployment by integrating multi-unit image batch processing, grid tile splitting, synthetic data augmentation, interactive dataset annotation, AutoML hyperparameter optimization, YOLO training/metrics tracking, and model export.

---

## 🛠️ Installation & Launch

Install the pre-compiled binary package directly using `pip`:

```bash
pip install visimac
```

Launch the workstation GUI directly from your terminal:

```bash
visimac
```

---

## 🚀 Workstation Guide

VisiMac Studio is designed as a sequential pipeline. The tabs at the top of the interface follow the natural progression of computer vision model development, from raw image processing to final model deployment.

### 1. Image Resizer + Compressor
Prepare raw, high-resolution source images for annotation or direct model ingestion without sacrificing aspect ratios or ballooning storage sizes.

* **Dimensional Scaling:** Resize entire directories using exact pixel bounds or physical print units (`cm`, `mm`, `in`, `pt`) mapped to target DPI settings. Include subfolders recursively to process large directory trees.
* **Exact Target Size Compression:** Check *"Enable Target Size Constraint"* and define a maximum file size in kilobytes (e.g., `500 KB`). The internal binary search algorithm calculates the mathematically optimal JPEG/WEBP quality factor for each individual image to hit the target file size without losing excess visual fidelity.
* **Format & Rotation Batching:** Convert image pools dynamically to JPEG, PNG, or WEBP while applying fixed orientation corrections (`90°`, `180°`, `270°`).
* **Live Queue Analytics:** Select any file in the processing queue to view a real-time estimation of the output resolution, DPI, and pre-compression source size.

### 2. Image Splitter
Decompose massive resolutions (such as satellite imagery, medical scans, or industrial panoramas) into manageable grids to maintain native detail during YOLO training.

* **Dynamic Grid Layout:** Specify the total number of target tiles. The engine calculates the optimal row-by-column matrix (e.g., entering `4` generates an auto-balanced 2 Rows x 2 Cols split).
* **Structured Outputs:** Toggle subfolder sorting to save each extracted tile into organized directories (e.g., `part_1/`, `part_2/`), preventing flat-folder clutter and aiding in parallel dataset construction.

### 3. Synthetic Data Enricher
Multiply your dataset variance and correct perspective distortions before object annotation.

* **Interactive Transformation Canvas:** Select an image and use the active mode dropdown to apply destructive spatial operations. Select **Crop** to isolate objects, **Warp** to map four corner points and flatten perspective distortion, or **Rotate** (fixed 90-degree steps or custom precision angles) to correct misaligned sensor data.
* **Augmentation Ranges:** Define strict Min/Max tolerances for visual variance, including:
  * Brightness Additions (`-100` to `+100` intensity offset)
  * Contrast Scaling (`0.1x` to `3.0x` multiplier)
  * Rotation Degrees (slight rotational variance)
  * Lighting/Gamma Curve Adjustments
  * Perspective Warp Skew Variables
* **Live Visual Simulation:** Click *"Simulate Augmentation Canvas"* to immediately preview how the engine (and subsequently YOLO) will distort your source data.
* **Batch Variant Generation:** Set *"Variants per Source"* to instantly synthesize and export multiple unique, randomized images from your defined ranges.

### 4. Image Annotator
A dedicated, distraction-free labeling environment for defining bounding boxes and segmentation masks.

* **Multi-Shape Draw Tools:** Trace targets using bounding Rectangles or segmentation Polygons, Circles, and Pentagons.
* **AI Auto-Labeling Assist:** Load pre-trained or previously generated YOLO model weights (`.pt`). Adjust the confidence threshold and click *"Auto-Label Frame"* to automatically draw bounds and masks over detected objects.
* **Sequence Frame-Copying:** Click *"Copy Previous Frame Shapes"* when labeling rapid video frames or stationary objects to instantly duplicate the prior image's exact coordinates.
* **Semi-Transparent Overlays:** Shapes are automatically filled with distinct, semi-transparent colors mapped to specific class IDs for clear overlap visibility. Selected shapes boost to 100% opacity for precise coordinate adjustments.
* **Dataset Health Inspector:** Run a comprehensive QA scan against the loaded directory. The inspector flags unannotated images, orphaned `.txt` files without matching images, and duplicate/overlapping annotations where the Intersection-over-Union (IoU) exceeds 90%.
* **Export & Split Dataset:** Select your target split ratio (e.g., `70/20/10` for Train/Val/Test). The engine automatically shuffles the data, copies images and labels to the appropriate subdirectories, standardizes YOLO coordinates, and generates a valid `data.yaml` manifest.

### 5. YOLO Configuration & AutoML
Define your model architecture and discover mathematically optimal training settings.

* **Model & Dataset Mapping:** Select from standard YOLO backbones (e.g., `yolov8n.pt`, `yolov8x-seg.pt`). Map the output directory and select the `data.yaml` generated in the previous step.
* **Manual Hyperparameters:** Configure Epochs, Batch Size, Input Image Size, Optimizer (`AdamW`, `SGD`, `Adam`), Data Loader Worker Threads, Initial/Final Learning Rates, and Mosaic thresholds.
* **AutoML Genetic Search:** Instead of guessing hyperparameter configurations, click *"Optimize Hyperparameters"*. This triggers a background routine that runs multiple abbreviated training lifecycles, cross-mutating variables to determine the mathematically optimal setup for your specific dataset, automatically applying the winning values back to the GUI.

### 6. Training Analytics & Live Logs
Monitor model convergence in real time without executing terminal commands.

* **Console Stream:** View real-time standard output and error logging directly inside the workstation.
* **Live Loss Curves:** The live plot graphs Train Box Loss, Validation Box Loss, and Train Class Loss dynamically as epochs complete.
* **Diagnostic Rendering:** Following training, visually inspect model performance via dedicated tabs for the generated F1-Score Curve, Precision-Recall Curve, Confusion Matrix, and overlaid Validation Batch Predictions.
* **Safe Halting:** Click *"Stop Training"* to gracefully conclude the training run at the end of the active epoch without corrupting the saved weights.

### 7. Inference & Model Export
Verify the newly trained model against isolated test data and compile it for production hardware.

* **Batch Inference:** Load your best `.pt` file and select a target folder of raw images. Dial in confidence thresholds and image input sizes.
* **Prediction Visualizer:** Navigate sequentially through the inference queue to review object detections, boundary precision, and classification confidence values drawn directly on the frames.
* **Production Deployment Export:** Select a target save directory and convert PyTorch weights (`.pt`) into optimized deployment formats:
  * `ONNX` (Cross-platform compatibility)
  * `engine` (NVIDIA TensorRT for maximum GPU acceleration)
  * `openvino` (Intel CPU/iGPU optimization)
  * `torchscript` (C++ production environments)
  * `coreml` (Apple Silicon iOS/macOS Neural Engine)

---

## 💻 Cross-Platform Support

| Operating System | Binary Extension Type | Architecture |
| :--- | :--- | :--- |
| **Windows** | `.pyd` | `x86_64` / `AMD64` |
| **Linux** | `.so` | `x86_64` |
| **macOS** | `.so` | `x86_64` / `arm64` (Apple Silicon) |
