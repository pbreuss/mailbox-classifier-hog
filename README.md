# Mailbox Classifier HOG

A lightweight image-classification project for detecting whether a smart mailbox contains mail.

The project uses **Histogram of Oriented Gradients (HOG)** features together with a **Random Forest classifier**. Training and experimentation are performed in a Jupyter notebook.

This project is part of a larger Smart Mailbox setup using ESP32-CAM devices and Home Assistant.

## Purpose

The goal is to automatically determine whether something has been delivered into the mailbox.

A camera mounted inside the mailbox captures an image. The classifier analyzes the image and predicts whether the mailbox is:

* Empty
* Contains mail or another object

The result can then be used by Home Assistant for notifications, status indicators, or other automations.

## How It Works

The basic pipeline is:

1. Collect mailbox images.
2. Preprocess the images.
3. Extract HOG features.
4. Train a Random Forest classifier.
5. Use the trained model to classify new mailbox images.
6. Forward the prediction to the smart-home system.

HOG is useful here because the camera position and mailbox environment are relatively controlled, making traditional computer-vision features a good fit for the problem.

## Repository Structure

The project is organized roughly like this:

```text
mailbox-classifier-hog/
├── train/
│   └── mailbox_random_forest_training.ipynb
├── data/
├── models/
├── requirements.txt
├── .gitignore
└── README.md
```

The exact contents may evolve as the project develops.

## Training

Training is performed in the Jupyter notebook:

```text
train/mailbox_random_forest_training.ipynb
```

The notebook contains the steps for:

* Loading the training images
* Preprocessing the dataset
* Extracting HOG features
* Training the Random Forest classifier
* Evaluating the classifier
* Saving the trained model

To start training, launch Jupyter from the project directory:

```bash
jupyter notebook
```

Then open:

```text
train/mailbox_random_forest_training.ipynb
```

and run the notebook cells in sequence.

## Virtual Environment

A local Python virtual environment can be used for the project.

For example:

```bash
python3 -m venv mailbox_classifier_venv
source mailbox_classifier_venv/bin/activate
```

The virtual environment itself should not be committed to Git.
