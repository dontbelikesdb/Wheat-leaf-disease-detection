# Wheat Disease Detection System

A Flask web application and TensorFlow training pipeline for classifying wheat images as healthy or affected by a disease. The inference model uses a ResNet50 backbone and accepts an uploaded image through a browser interface.

## Supported classes

- Fusarium Head Blight
- Healthy Wheat
- Leaf Rust
- Tan Spot
- Unknown

## Project structure

```text
app.py              Flask application for image uploads and predictions
train.py            ResNet50 training and fine-tuning pipeline
test_image.py       Command-line checks using sample images
create_labels.py    Utility for generating labels
templates/          HTML pages for the web interface
static/             Images and other static web assets
requirements.txt    Python dependencies
```

## Prerequisites

- Python 3.10 or later
- A trained `model.h5` file and matching `lb.pickle` label binarizer in the project root

For training, place images under `Dataset/` using one directory per class:

```text
Dataset/
|-- Fusarium Head Blight/
|-- Healthy Wheat/
|-- Leaf Rust/
|-- Tan Spot/
`-- Unknown/
```

The dataset and model artifacts are excluded from Git because they can be large. Obtain them separately or generate them by running the training script.

## Installation

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
```

## Run the web application

Ensure `model.h5` and `lb.pickle` are available in the project root, then run:

```powershell
python app.py
```

Open <http://127.0.0.1:5000> in a browser, upload a wheat image, and review the predicted class with its confidence score.

`run.bat` is also provided as a Windows launcher; update its Python path if it differs from your local installation.

## Train a model

With the dataset in the expected folder layout, run:

```powershell
python train.py
```

The script splits the dataset into training and validation sets, augments training images, trains the classification head, fine-tunes the final ResNet50 layers, and writes these generated files:

- `model.h5` - final trained model
- `best_model.h5` - checkpoint with the best validation accuracy
- `lb.pickle` - fitted label binarizer
- `classification_report.json` - validation metrics
- `confusion_matrix.csv` - validation confusion matrix

## Test model predictions

Run the sample-image checks with:

```powershell
python test_image.py
```

This script evaluates one available image from each dataset class and prints the predicted class and confidence.

## Notes

- Images are resized to `224 x 224` pixels before inference.
- Basic validation rejects images that are extremely dark, bright, or nearly a solid colour.
- A prediction is informational only and should not replace agronomic or plant-pathology advice.
