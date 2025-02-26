# Replication of Does Knowledge Distillation Really Work?

This repo aims to replicate the experiments in the paper "Does Knowledge Distillation Really Work?".

## Introduction

### Trained Models (if only distillation will be done):

Trained models can be accessed from the "trained_models" folder. 

### Resulting Files: 

All the results can be accessed from the "all_resulting_csv_files" folder. 

- Figure 1 files: self_cifar_without_gan.csv, self_gan_12.5.csv, self_gan_25.csv, self_gan_37.5.csv, self_gan_50.csv,
ensemble_cifar_without_gan.csv, ensemble_gan_12.5.csv, ensemble_gan_25.csv, ensemble_gan_37.5.csv, ensemble_gan_50.csv

- Figure 2 files: mnist.csv, emnist_175k.csv, emnist_350k.csv

- Figure 3 files: baseline_with_temperature_4.csv, baseline_with_temperature_1.csv, mixup_data_augmentation_temperature_1.csv, rotation_with_temperature_1.csv

- Figure 5 (left) files: same as Figure 1

- Figure 6 (a) files: adam.csv, sgd.csv

## How to Set Up the Environment?

We used Google Colab in our experiments. To be able to run the code provided by the authors: [gnosis repository](https://github.com/samuelstanton/gnosis), you first need to set the environment up. To do that, run the following code blocks in Colab:

```bash
# Install Python 3.8 and virtual environment tools
sudo apt-get install python3.8 python3.8-venv python3.8-dev

# Create clean virtual environment
python3.8 -m venv --clear gnosis-env
source gnosis-env/bin/activate

# Clone repo with fresh copy
rm -rf gnosis
git clone https://github.com/samuelstanton/gnosis.git
cd gnosis

# Fix dependencies for Python 3.8 compatibility
sed -i 's/sklearn/scikit-learn/' requirements.txt

# Install core build dependencies
pip install --upgrade "pip<23.3.1" setuptools==59.5.0 wheel

# Install PyTorch with CUDA 11.1 (matching Torch 1.10)
pip install torch==1.10.0+cu113 torchvision==0.11.1+cu113 torchaudio==0.10.0+cu113 -f https://download.pytorch.org/whl/cu113/torch_stable.html

# Install remaining requirements with numpy first
pip install "numpy<1.24"
pip install -r requirements.txt --ignore-installed
pip install -e .
```

and the following: 


```bash
%%bash
# Activate the virtual environment
source gnosis-env/bin/activate
 
# Downgrade protobuf to version 3.20.3
pip install protobuf==3.20.3
```

## How to Run the Experiments?

Use the following commands after setting the environment:

- For Figure 1: 
```bash
```
- For Figure 2: 
```bash
```
- For Figure 3: 
```bash
```
- For Figure 5 (left:
```bash
```
Same as figure 1 
```bash
```
- For Figure 6 (a):
```bash
```
