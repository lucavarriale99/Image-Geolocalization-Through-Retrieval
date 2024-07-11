# Image Geolocalization Through Retrieval 

Visual geo-localization (VG), also known as Visual Place Recognition (VPR), involves determining the location depicted in a query image by referring to a database of geo-tagged reference images. The top retrieved images, along with their geo-tags (typically GPS coordinates), provide hypotheses for the query’s geographical location. In our [work](https://github.com/lucavarriale99/Image-Geolocalization-Through-Retrieval/blob/main/Exploring%20Visual%20Place%20Recognition.pdf), we investigate how the combination of different miners, loss functions, and aggregator layers performs on challenging datasets. We also introduce a new aggregator layer, ConvGeM, which leverages key features of state-of-the-art aggregators. Furthermore, we emphasize and formalize how the domain and dataset significantly influence the choice of the "right" model and which components best fit specific contexts.

For further information, please refer to the [paper](https://github.com/lucavarriale99/Image-Geolocalization-Through-Retrieval/blob/main/Exploring%20Visual%20Place%20Recognition.pdf).

Below are the steps to proceed with the experiments.

## Run an experiment

This repository serves as a starting point to implement a VPR pipeline. It allows you to train a simple
ResNet-18 on the GSV dataset. It relies on the [pytorch_metric_learning](https://kevinmusgrave.github.io/pytorch-metric-learning/)
library.

You can choose to validate/test on sf_xs or tokyo_xs.

```bash
python main.py --exp_name example_run --train_path /path/to/datasets/gsv_xs --val_path /path/to/datasets/sf_xs/val --test_path /path/to/datasets/sf_xs/test
```

## Test on a pretrained model

In every experiment the best model, chosen by evaluating the R@1, and the last model are saved in the **./logs/lightning_logs/\<exp_name\>/checkpoints/** folder in 2 distinct **.ckpt** files.  

We can use these checkpoint files to test the model against a new test dataset.  

To test the pretrained model on new test dataset you can simply add the **--ckpt_path** arg, indicating the path to a .ckpt file, when executing the main.  
This will **not** trigger the training and the validation phases but will only test the model on the dataset indicated in the --test_path arg.  

```bash
python main.py --ckpt_path ./logs/lightning_logs/\<exp_name\>/checkpoints/last.ckpt \
  --train_path /content/gsv_xs --val_path /content/sf_xs/val --test_path /content/tokyo_xs/test
```

## Continue a model training

We can use the checkpoints files to resume the training from the last checkpoint:

```bash
python main_ckpt.py --ckpt_path ./logs/lightning_logs/<exp_name>/checkpoints/last.ckpt \
  --train_path /content/gsv_xs --val_path /content/sf_xs/val --test_path /content/tokyo_xs/test
```

Take care about selecting all the same parameters of the initial training in case you want to continue the training.  

## Select Aggregator Layer and Loss

It is possible to pass specific args to select the Aggregator Layer, the Miner and the Loss we want to use to train the model.  

```bash
python main.py --train_path /content/gsv_xs --val_path /content/sf_xs/val --test_path /content/sf_xs/test \
  --num_workers 2 --loss_name MultiSimilarityLoss --agg_arch ConvAP
```

It is possible to choose among multiple types of Aggregator Layers, Miners and Losses, for a complete list check:

- the file [./utils/losses.py](https://github.com/danielemansillo/Simple_VPR_codebase/blob/main/utils/losses.py) for Miners and Losses
- the folder [./models/aggregators/](https://github.com/danielemansillo/Simple_VPR_codebase/tree/main/models/aggregators) for the Aggregator Layers

## Usage on Colab

We provide the notebook `colab_example.ipynb`.
It shows you how to attach your GDrive file system to Colab, unzip the datasets, install packages and run your first experiment.
