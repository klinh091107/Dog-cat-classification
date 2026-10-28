# Dog Cat Classification

This project classifies images of cats and dogs and compares three neural networks: a multilayer perceptron (MLP), VGG16, and ResNet18. The notebook includes data preprocessing, model training, validation, and a Gradio interface for comparing predictions on an uploaded image.

## Contributors

- [klinh091107](https://github.com/klinh091107)

## Installation

1. Clone this repository:

   ```bash
   git clone https://github.com/klinh091107/dog-cat-classification.git
   cd dog-cat-classification
   ```

2. Create a Python environment and install the required libraries:

   ```bash
   python -m venv .venv
   source .venv/bin/activate
   # On Windows, use: .venv\Scripts\activate
   pip install -r requirements.txt
   ```

   The dependencies are unpinned because the original notebook does not record exact package versions. A GPU is recommended for training; internet access is needed to download pretrained weights.

3. Download the [Cat & Dog Images for Classification dataset](https://www.kaggle.com/datasets/ashfakyeafi/cat-dog-images-for-classification). Arrange the extracted files as follows:

   ```text
   dataset-root/
   ├── cat_dog.csv       # Contains the image and labels columns
   └── cat_dog/          # Images referenced by the CSV
   ```

   Set `data_dir` in the second code cell to your dataset directory. The existing path targets Kaggle:

   ```python
   data_dir = Path("/path/to/dataset-root")
   ```

   Alternatively, import the notebook into Kaggle, attach the dataset, and check that `data_dir` matches its mounted path. The notebook creates stratified train/validation/test splits of 70%/10%/20%, using random seed `845`.

4. Open the notebook and run its cells in order:

   ```bash
   jupyter lab dog-cat-classification.ipynb
   ```

   The MLP uses normalized 64 × 64 images. VGG16 and ResNet18 use 224 × 224 images with model-specific preprocessing and frozen ImageNet-pretrained backbones. Training uses binary cross-entropy and early stopping on validation loss, restoring the best weights.

   The final cells launch a Gradio interface with predictions from all three models. The current `share=True` setting creates a temporary public demo link; use `share=False` for a local-only interface. Models are trained in memory, so a fresh session must run the training cells before the demo.

## Structure

```text
dog-cat-classification/
├── dog-cat-classification.ipynb  # Preprocessing, training, evaluation, and demo
├── requirements.txt             # Python dependencies
├── README.md                    # Setup instructions and recorded results
└── .gitignore                   # Excludes local data, environments, and weights
```

## Result

The following validation results are recorded in the notebook after restoring the best weights:

| Model | Accuracy | Precision | Recall | Loss |
| --- | ---: | ---: | ---: | ---: || MLP | 63.96% | 63.33% | 66.32% | 0.6419 |
| VGG16 | 98.28% | 97.56% | 99.04% | 0.0506 |
| ResNet18 | 98.92% | 99.04% | 98.80% | 0.0352 |

ResNet18 has the highest recorded validation accuracy. These numbers come from saved notebook outputs, not a fresh reproduction. Although the notebook prepares a test split, it does not evaluate the models on that split; these are not test-set results. The Gradio percentages represent prediction probabilities, not model accuracy.

