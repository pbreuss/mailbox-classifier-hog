# Mailbox Classifier HOG

A lightweight image-classification project for detecting whether a mailbox contains mail.

The classifier uses **Histogram of Oriented Gradients (HOG)** features together with a traditional machine-learning classifier, making it suitable for small embedded and smart-home projects where a full deep-learning model would be unnecessary or too resource-intensive.

This project is part of a larger **Smart Mailbox** setup using ESP32-CAM devices, Home Assistant, and camera-based automation.

## Purpose

The goal is to automatically determine whether something has been delivered into the mailbox.

A camera mounted inside the mailbox captures an image. The classifier then analyzes the image and determines whether the mailbox is:

* Empty
* Contains mail or another object

The result can then be used by Home Assistant to trigger automations such as notifications or status indicators.

## How It Works

The basic processing pipeline is:

1. Capture an image of the mailbox.
2. Resize and preprocess the image.
3. Extract HOG features.
4. Pass the feature vector to the trained classifier.
5. Predict whether the mailbox is empty or contains mail.

HOG works well for this use case because it describes the shapes, edges, and structures visible in an image without requiring a computationally expensive neural network.

## Smart Mailbox Architecture

The classifier is intended to work as part of a larger setup:

```text
ESP32-CAM
    |
    | image
    v
Mailbox Classifier
    |
    | prediction
    v
Home Assistant
    |
    +--> Mail notification
    +--> Mailbox status
    +--> LED indicator
    +--> Smart-home automation
```

The ESP32-CAM provides the mailbox image, while the actual image classification can run on a more capable device such as a server, NAS, Raspberry Pi, or Home Assistant host.

## Technologies

The project uses technologies such as:

* Python
* OpenCV
* HOG — Histogram of Oriented Gradients
* scikit-learn
* ESP32-CAM
* Home Assistant

## Example Project Structure

```text
mailbox-classifier-hog/
├── src/
│   ├── train.py
│   ├── predict.py
│   └── features.py
│
├── data/
│   ├── empty/
│   └── mail/
│
├── models/
│
├── requirements.txt
├── .gitignore
└── README.md
```

The exact structure may differ depending on the current version of the project.

## Installation

Clone the repository:

```bash
git clone https://github.com/YOUR-USERNAME/mailbox-classifier-hog.git
cd mailbox-classifier-hog
```

Create a virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install the required Python packages:

```bash
pip install -r requirements.txt
```

## Training

Training images should be divided into two categories, for example:

```text
data/
├── empty/
└── mail/
```

Images from both categories are processed and converted into HOG feature vectors.

The classifier is then trained on these features and stored as a model file for later predictions.

Example:

```bash
python src/train.py
```

## Prediction

To classify a new mailbox image:

```bash
python src/predict.py path/to/image.jpg
```

A typical result might look like:

```text
Prediction: MAIL
Confidence: 0.94
```

or:

```text
Prediction: EMPTY
Confidence: 0.97
```

The actual command-line interface depends on the current implementation.

## Why HOG?

Modern image classification is often based on deep-learning models, but that is not always necessary.

For a controlled environment such as the inside of a mailbox, HOG has several advantages:

* Very lightweight
* Fast inference
* Small model size
* Requires relatively little training data
* Runs easily on CPUs
* Easy to train and debug
* No GPU required

Because the camera position and mailbox environment remain largely constant, traditional computer-vision techniques can work surprisingly well.

## Home Assistant Integration

The prediction result can be exposed to Home Assistant as a sensor or event.

For example:

```text
binary_sensor.mailbox_contains_mail
```

Home Assistant can then trigger an automation when the state changes from empty to occupied.

Possible automations include:

* Send a push notification
* Turn on a green mailbox LED
* Display mailbox status on a dashboard
* Store the detection image
* Trigger additional camera recording
* Reset the status when the mailbox is emptied

## Smart Mailbox Project

This classifier is one component of a larger DIY Smart Mailbox system that combines:

* ESP32-CAM cameras
* Mailbox illumination
* Door and mail-slot sensors
* Motion detection
* Home Assistant
* NAS-based video storage
* Camera notifications
* HomeKit integration
* Computer-vision-based mail detection

The aim is to make the mailbox remotely observable while keeping the system inexpensive, local, and privacy-friendly.

## Status

Work in progress.

The project is currently being developed and tested with images captured from a real mailbox installation.

## License

Add the license of your choice here, for example MIT.
