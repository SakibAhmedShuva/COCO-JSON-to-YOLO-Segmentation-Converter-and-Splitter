# COCO JSON to YOLO Segmentation Converter and Splitter

A robust Python utility for converting COCO format annotations to YOLO format and automatically splitting the dataset into train/validation/test sets. Features full support for instance segmentation masks and bounding boxes, compatible with YOLOv8 and YOLOv11 segmentation models.

## 🔍 Key Features

- **Automatic Dataset Splitting**: Splits data into train/validation/test sets with configurable ratios
- **Full Segmentation Support**: Converts COCO polygon segmentation masks to YOLO format
- **Bounding Box Support**: Also handles traditional bounding box annotations
- **YOLOv8/v11 Compatible**: Generated annotations work with latest YOLO versions
- **Automatic data.yaml Generation**: Creates required YAML configuration file
- **Progress Tracking**: Uses tqdm for conversion progress visualization
- **Maintains Directory Structure**: Organizes output in YOLO-compatible directory structure

## 📋 Prerequisites

```bash
pip install -r requirements.txt
```

## 📁 Output Structure

```
output_directory/
├── train/
│   ├── images/
│   │   ├── image1.jpg
│   │   └── ...
│   └── labels/
│       ├── image1.txt
│       └── ...
├── valid/
│   ├── images/
│   │   ├── image2.jpg
│   │   └── ...
│   └── labels/
│       ├── image2.txt
│       └── ...
├── test/  # (optional)
│   ├── images/
│   │   ├── image3.jpg
│   │   └── ...
│   └── labels/
│       ├── image3.txt
│       └── ...
└── data.yaml
```

Sample data provided in the below folders:
- ./annotations
- ./images

## 📝 Output Format

### Segmentation Format
For segmentation masks, each line in the label files follows the format:
```
<class_id> <x1> <y1> <x2> <y2> ... <xn> <yn>
```
Where:
- `class_id`: Integer class identifier (0-based)
- `x1, y1, x2, y2, ..., xn, yn`: Normalized polygon coordinates (0-1)

### Bounding Box Format
For bounding boxes, each line follows the standard YOLO format:
```
<class_id> <x_center> <y_center> <width> <height>
```

## ⚙️ Configuration

### Data Splitting
The converter supports flexible dataset splitting with configurable ratios:
```python
coco_to_yolo(
    coco_json_path='path/to/annotations.json',
    image_dir='path/to/images',
    output_dir='yolo-output',
    train_ratio=0.8,    # 80% for training
    val_ratio=0.2,      # 20% for validation
    test_ratio=0.0,     # Optional test set
    seed=42            # For reproducible splits
)
```

### YAML Configuration
The script automatically generates a `data.yaml` file containing:
- Training, validation, and test set paths
- Number of classes
- Class names

## 🔰 Important Notes

1. **Dataset Splitting**: Random but deterministic splitting based on provided seed
2. **Segmentation Priority**: The converter prioritizes segmentation masks over bounding boxes when both are present
3. **Coordinate Normalization**: All coordinates are automatically normalized to the 0-1 range
4. **Instance Handling**: Properly handles multiple instances per image
5. **RLE Support**: Can process both polygon and RLE mask formats
6. **Crowd Annotations**: Filters out crowd annotations (iscrowd=1)

## ⚠️ Requirements

- Python 3.6+
- pycocotools
- tqdm

## 📈 Performance

- Efficiently handles large datasets
- Memory-optimized for processing large annotation files
- Progress bar indicates conversion status
- Maintains data distribution across splits

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- COCO dataset team for the annotation format
- Ultralytics for YOLO format specifications
