# OpenImages

<p align="center">
  <img src="https://github.com/kevinpruvost/OpenImages/blob/miscellaneous/images/1200px-Tsinghua_University_Logo.svg.png" width=250/><br/><br/>
  A kaggle competition we made in a group of 3 for the Tsinghua University's Machine Learning course.
</p>

# Introduction

OpenImages is originally a Kaggle competition where contestants had to program an AI with the best Object Detection Track capabilities.

By doing so, we have to work on Computer Vision and Deep Learning notions to achieve this goal.

Kaggle Competition : https://www.kaggle.com/c/open-images-2019-object-detection/overview

# Team Composition

| Name                        | Chinese Name | Github Profile                          |
|-----------------------------|--------------|-----------------------------------------|
| Pruvost Kevin (Team Leader) | 潘凱文       | [Link](https://github.com/kevinpruvost) |
| Carus                       | 屈颖钢       | [Link](https://github.com/Carusy)                                |
| Renard Lucas                | 雷纳卢卡     | [Link](https://github.com/LightFox7)    |

# YOLOv5 tutorial

## Download

- Create a folder and `cd` into it:
- Clone our custom YOLOv5 repository and checkout on the `darknet-yolov5` branch:

```powershell
git clone git@github.com:kevinpruvost/OpenImages.git -b darknet-yolov5
```

- Clone our custom COCO Dataset settings (and don't forget to rename the folder `coco` :

```powershell
git clone git@github.com:kevinpruvost/CocoDataset_KFolded.git coco
```

- Download the Train, Val & Test images from the 2017 dataset.

[COCO - Common Objects in Context](https://cocodataset.org/#download)

If the download links on [cocodataset.org](http://cocodataset.org) don't work, then create a new tab and copy paste these links (for some reason, opening the links directly in new tabs doesn't work, at least on Google Chrome) :

- [http://images.cocodataset.org/zips/train2017.zip](http://images.cocodataset.org/zips/train2017.zip)
- [http://images.cocodataset.org/zips/val2017.zip](http://images.cocodataset.org/zips/val2017.zip)
- [http://images.cocodataset.org/zips/test2017.zip](http://images.cocodataset.org/zips/test2017.zip)
- Then create a folder named `images` inside `coco` and place the 3 images folder in it like so:

```bash
.
├───coco
│   ├───annotations
│   ├───coco128
│   │   ├───images
│   │   │   └───train2017
│   │   └───labels
│   │       └───train2017
**│   ├───images
│   │   ├───test2017
│   │   ├───train2017
│   │   └───val2017**
│   └───labels
│       ├───train2017
│       └───val2017
├───OpenImages
│   ├───Project Proposal
│   │   └───__MACOSX
│   └───yolov5
|   ...
```

## Installation

- First, you can install the requirements from the `requirements.txt` from the `yolov5` folder.

```bash
cd OpenImages/yolov5
pip install -r requirements.txt
```

- If you got a good enough internet connection, don't forget to setup wandb.ai, so that we will get statistics of the training part for the final report:

[YOLOv5](https://docs.wandb.ai/guides/integrations/yolov5)

## Training

### K Fold Cross Validation

For this, part, as K Fold Cross Validation seems to improve the results quite a lot, then we will perform this method with a K of 3.

So with the COCO training + validation dataset, we got 118K + 5K = ~123K images.

With the 3 Folded configuration, we then got 3 folds of equal size :

82K for Training & 41K for Validation.

The 3 Folds are generated randomly and their configuration is located in:

- `coco/train2017_fold0.txt` `coco/train2017_fold1.txt` `coco/train2017_fold0.txt`
- `coco/val2017_fold0.txt` `coco/val2017_fold1.txt` `coco/val2017_fold2.txt`

With cache pre generated.

### Epochs

For the number of epochs, many experiments have been conducted by other people and the consensus is that 300 epochs is the most optimized parameter. So it will take a long time for us to train that.

### Batch Size

The batch size has to be changed depending on the capacities of your GPU, if you have more VRAM, then you'll be able to handle more batches.

The overall thought about batch sizes is that, the more the batches, the better it is. I don't really know why.

I am personally using `8` for the batch size.

### Image Size

`640` seems to be the best parameter for us.

Basically, image size allows YOLOv5 for more accurate training but the bigger the image size is, the slower it gets.

But if we go lower than `640` , then the results get quite bad.

### Workers

Supposed to be the number of cores used by the program.

Though, on YOLOv5, higher count of workers seem to be quite problematic, I got 8 cores on my CPU but I tend to have more stable results with only 2-4 workers. I prefer staying at 2 workers.

### Device

If you want to assign your GPU manually, then specify the flag `--device 0`

### Weights

As we are performing Fine-Tuning, we **MUST** use the `yolov5s.pt` weights.

### My experiments

[pruvostkevin0](https://wandb.ai/pruvostkevin0/train)

### The Competition

[Competition](https://competitions.codalab.org/competitions/20794#results)

### Command to Launch

- To start the training:

<aside>
⚠️ DO NOT FORGET TO CHANGE THE `--data` AND `--project` FLAGS TO YOUR ASSIGNED FOLD

</aside>

```python
python.exe .\train.py --img 640  --batch 8 --epochs 300 --project fold1 --data .\data\coco_fold1.yaml --weights .\models\yolov5s.pt --workers 2 --device 0
```

- To resume the training (if you had to stop):

```python
python.exe .\train.py --resume
```

## After Training

Basically, you will have to push your model on the Github repository, so when you're done, just add this folder:

```bash
./OpenImages/yolov5/runs/train/fold1 or 2 or 3
```

## If you want to test your model

To test a dataset:

[Google Colaboratory](https://colab.research.google.com/github/ultralytics/yolov5/blob/master/tutorial.ipynb#scrollTo=rc_KbFk0juX2)

To test a single image, or webcam, or video, or...

[Google Colaboratory](https://colab.research.google.com/github/ultralytics/yolov5/blob/master/tutorial.ipynb#scrollTo=4JnkELT0cIJg)

### Making tests on a dataset

```bash
python val.py --weights fold1/exp3/weights/last.pt fold2/exp/weights/last.pt fold3/exp2/weights/last.pt --data data/coco_fold1.yaml --batch-size 2 --imgsz 640 --save-json
```

### Exporting results to the COCO contest site

- Take the json file generated by val.py `best_predictions.json`
- Zip it
- Rename the zip file to `detections_$(SET_NAME)_$(TEAM_NAME)_results.zip`
    - `$(SET_NAME)` can either be `test-dev2017` or `val2017`.
