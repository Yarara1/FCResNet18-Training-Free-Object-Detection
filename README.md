# FCResNet18-Training-Free-Object-Detection
# Training-Free Object Detection with FCResNet18

This project converts an **ImageNet-pretrained ResNet-18** into a fully convolutional network for **training-free object localization**.

Instead of retraining an object detector, the pretrained classifier is modified to generate spatial class-response maps that highlight regions associated with a predicted ImageNet class.

## Method

The original ResNet-18 is modified by:

- Replacing the initial max pooling with average pooling
- Removing global average pooling
- Replacing the final fully connected classifier with a `1×1` convolution
- Reusing the pretrained ImageNet classifier weights in the new convolution layer

The resulting FCResNet18 accepts arbitrary-sized images and produces:

<img width="513" height="531" alt="image" src="https://github.com/user-attachments/assets/589a055a-a5b9-46ed-8d5a-95d230b04191" />


No additional model training is required.

## Object Localization

For each image:

1. Run inference with FCResNet18.
2. Extract the response map for the target or most activated class.
3. Resize the response map to the original image resolution.
4. Apply relative thresholding.
5. Detect the activated region and generate a bounding box.

## Results

### Arabian Camel

- Input size: `1536 × 580`
- Response map: `19 × 48`
- ImageNet class: `354 — Arabian camel`

The response map strongly activated around the camel's head and upper body. The resulting bounding box localized the most discriminative region rather than the entire object.

### Limpkin

- Input size: `480 × 360`
- Class: `135 — limpkin`

This produced one of the strongest localization results, with activation covering most of the bird's body.

### Red-backed Sandpiper

- Input size: `850 × 607`
- Class: `140 — red-backed sandpiper`

The response was concentrated mainly around the bird's head, resulting in only partial-object localization.

## Out-of-Distribution Images

The detector was also tested on a face, concert-hall image, and cake image.

Because ResNet-18 was trained on ImageNet classification rather than general object detection, these images produced unrelated class predictions such as:

```text
Face       → Knee pad
Concert    → Panpipe
Cake       → Windsor tie
```

The corresponding bounding boxes often highlighted textures or edges rather than meaningful objects.

## Limitations

- ResNet-18 is trained for classification rather than bounding-box detection.
- Response maps have relatively low spatial resolution.
- Localization often focuses on the most discriminative object part.
- Detection is limited to ImageNet-related visual features.
- Results are sensitive to threshold and morphology settings.

## Possible Improvements

- Grad-CAM / CAM visualization
- Fine-tuning using bounding-box annotations
- Higher-resolution feature maps
- Multi-scale feature fusion
- Dedicated detectors such as YOLO, SSD, or Faster R-CNN

## Technologies

`Python` · `PyTorch` · `ResNet-18` · `OpenCV` · `Computer Vision` · `Fully Convolutional Networks`
