# Super Resolution for scanned text images

The tinny model to upscale scanned images with text. The model used `ConvTranspose2d` layer instead of `PixelShuffle` as result the
model can be launched on GPU and MYRIAD devices and Inference Engine support `reshape` function.

## Train and evaluation

### Prepare dataset
Create two directories for train and test images. Train images may have any resolution more than `path_size`.
Validation images should have resolution like `path_size`.

```
./data
├── train
│   ├── 000000.png
│   ...
└── val
    ├── 000000.png
    ...
```
Image should be in gray scale format and contain only black (0) and white (255) pixels

**NOTE** Better use cropped images (for example: 500x500), because large resolution
dramatically increase time to reading images


### Training

Use `tools/train.py` script to start training process:
```
python3 tools/train.py --config configs/text_scale3.yaml
```

To start from pretrained checkpoint set `init_checkpoint` in config.
Checkpoints can be downloaded [here](https://download.01.org/opencv/openvino_training_extensions/models/super_resolution/text_super_resolution.tar.gz).

### Testing

Use `tools/test.py` script to evaluate the trained model.

```
python3 tools/test.py --test_data_path PATH_TO_TEST_DATA \
    --models_path PATH_TO_MODELS_PATH \
    --exp_name EXPERIMENT_NAME
```

## Export to OpenVINO
```
python3 tools/export.py --models_path PATH_TO_MODELS_PATH \
    --exp_name EXPERIMENT_NAME \
    --input_size 200 200 \
    --data_type FP32
```

## Demo

### For the latest checkpoint
```
python3 tools/text/infer.py --model PATH_TO_CHECKPOINT IMAGE_PATH
```

### For Intermediate Representation (IR)
```
python3 tools/text/infer_ie.py --model <PATH_TO_IR_XML> \
    --device CPU \
    image_path
```
