# Mailbox Classifier – Home Assistant App

Version 0.2.0 uses **HOG + color features with a Random Forest**.

This gives the classifier both:
- color/brightness information
- structural/spatial information from HOG

That is useful for difficult cases such as a white letter or paper that has similar overall
brightness to the empty mailbox but changes local edges and structure.

## API

### POST `/classify`

```json
{
  "image": "/media/current_mailbox.jpg"
}
```

Example:

```json
{
  "ok": true,
  "classifier": "hog_color_random_forest",
  "prediction": "occupied",
  "occupied_probability": 0.94,
  "empty_probability": 0.06
}
```

### GET `/health`

Shows whether the model is available.

### POST `/reload`

Reloads the model from disk without restarting the App.

## Model

Default path:

```text
/data/mailbox_rf.joblib
```

Train it with:

```text
training/mailbox_random_forest_training.ipynb
```

## Dataset

```text
dataset/
├── empty/
└── occupied/
```

## Feature set

Color:
- RGB histograms
- HSV histograms
- grayscale histogram
- RGB mean/std
- grayscale mean/std
- simple edge-density

Structure:
- HOG on a 160 × 120 grayscale image
- 9 orientations
- 8 × 8 pixels per cell
- 2 × 2 cells per block
- L2-Hys normalization

The training notebook and App intentionally use identical extraction parameters.

## Home Assistant REST command

```yaml
rest_command:
  mailbox_classifier:
    url: "http://local-mailbox-classifier:8098/classify"
    method: POST
    content_type: "application/json"
    payload: >
      {
        "image": "{{ image }}"
      }
```

Automation:

```yaml
- action: rest_command.mailbox_classifier
  data:
    image: "/media/current_mailbox.jpg"
  response_variable: classifier_result

- variables:
    prediction: "{{ classifier_result.content.prediction }}"
    occupied_probability: "{{ classifier_result.content.occupied_probability | float }}"
```
