---
comments: true
---

# Instance Segmentation Datasets Overview

## Supported Dataset Formats

### Ultralytics YOLO format

** Label Format **

The dataset format used for training YOLO segmentation models is as follows:

1. One text file per image: Each image in the dataset has a corresponding text file with the same name as the image file and the ".txt" extension.
2. One row per object: Each row in the text file corresponds to one object instance in the image.
3. Object information per row: Each row contains the following information about the object instance:
   - Object class index: An integer representing the class of the object (e.g., 0 for person, 1 for car, etc.).
   - Object bounding coordinates: The bounding coordinates around the mask area, normalized to be between 0 and 1.

The format for a single row in the segmentation dataset file is as follows:

```
<class-index> <x1> <y1> <x2> <y2> ... <xn> <yn>
```

In this format, `<class-index>` is the index of the class for the object, and `<x1> <y1> <x2> <y2> ... <xn> <yn>` are the bounding coordinates of the object's segmentation mask. The coordinates are separated by spaces. 

Here is an example of the YOLO dataset format for a single image with two object instances:

```
0 0.6812 0.48541 0.67 0.4875 0.67656 0.487 0.675 0.489 0.66
1 0.5046 0.0 0.5015 0.004 0.4984 0.00416 0.4937 0.010 0.492 0.0104
```
Note: The length of each row does not have to be equal.

** Dataset file format **

The Ultralytics framework uses a YAML file format to define the dataset and model configuration for training Detection Models. Here is an example of the YAML format used for defining a detection dataset:

```yaml
train: <path-to-training-images>
val: <path-to-validation-images>

nc: <number-of-classes>
names: [<class-1>, <class-2>, ..., <class-n>]

```

The `train` and `val` fields specify the paths to the directories containing the training and validation images, respectively.

The `nc` field specifies the number of object classes in the dataset.

The `names` field is a list of the names of the object classes. The order of the names should match the order of the object class indices in the YOLO dataset files.

NOTE: Either `nc` or `names` must be defined. Defining both are not mandatory.

Alternatively, you can directly define class names like this:
```yaml
names:
  0: person
  1: bicycle
```

** Example **

```yaml
train: data/train/
val: data/val/

nc: 2
names: ['person', 'car']
```

## Usage
!!! example ""

    === "Python"
    
        ```python
        from ultralytics import YOLO
        
        # Load a model
        model = YOLO('yolov8n-seg.pt')  # load a pretrained model (recommended for training)

        # Train the model
        model.train(data='coco128-seg.yaml', epochs=100, imgsz=640)
        ```
    === "CLI"
    
        ```bash
        # Start training from a pretrained *.pt model
        yolo detect train data=coco128-seg.yaml model=yolov8n-seg.pt epochs=100 imgsz=640
        ```

## Supported Datasets

## Port or Convert label formats

### COCO dataset format to YOLO format

```
from ultralytics.yolo.data.converter import convert_coco

convert_coco(labels_dir='../coco/annotations/', use_segments=True)
```
