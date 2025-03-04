# Replication of "Does Knowledge Distillation Really Work?"

This repo aims to replicate the experiments in the paper "Does Knowledge Distillation Really Work?" https://arxiv.org/abs/2106.05945.

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

### Scripts

Scripts can be accessed from the "scripts" folder. 

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

  
To train the teachers (trial_id (seed) can be changed, if you want to train 3 teachers at once, you can give three numbers like 1,2,3):
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m teacher.use_ckpts=False classifier.depth=56 \
  trainer.num_epochs=50 trainer.optimizer.lr=5e-2 \
  trainer.lr_scheduler.eta_min=0. \
  trainer.distill_teacher=False dataloader.batch_size=256 \
  trial_id=0
```
Self distillation without GAN:
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m trial_id=3 exp_name=student_resnet_56_self_distil \
  classifier.depth=56 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  trainer.lr_scheduler.eta_min=0. \
  trainer.num_epochs=20 \
  teacher.ckpt_dir="/content/content/content/data/experiments/image_classification/test/preresnet56_cifar100_unitcube/leather_sofa_3"
```
Ensemble distillation without GAN:
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m trial_id=3 exp_name=student_resnet_56_self_distil \
  classifier.depth=56 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  trainer.lr_scheduler.eta_min=0. \
  trainer.num_epochs=20 \
  teacher.ckpt_dir="/content/content/content/data/experiments/image_classification/test/preresnet56_cifar100_unitcube"
```
To train the GAN: 
```bash
!source gnosis-env/bin/activate && python /content/gnosis/scripts/image_generation.py \
  -m trainer.checkpoint_period=1 \
  hydra.run.dir="/content/checkpoints" \
  logger.log_dir="/content/checkpoints" \
  data_dir="/content/checkpoints" \
  trainer.eval_period=100
```
Self distillation with 12.5k GAN:
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m trial_id=3 exp_name=student_resnet_56_self_distil \
  classifier.depth=56 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  trainer.lr_scheduler.eta_min=0. \
  distill_loader.synth_ratio=0.2 \
  trainer.num_epochs=20 \
  teacher.ckpt_dir="/content/content/content/data/experiments/image_classification/test/preresnet56_cifar100_unitcube/leather_sofa_3"
```
Ensemble distillation with 12.5k GAN:
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m trial_id=3 exp_name=student_resnet_56_self_distil \
  classifier.depth=56 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  trainer.lr_scheduler.eta_min=0. \
  distill_loader.synth_ratio=0.2 \
  trainer.num_epochs=20 \
  teacher.ckpt_dir="/content/content/content/data/experiments/image_classification/test/preresnet56_cifar100_unitcube"
```
Self distillation with 25k GAN:
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m trial_id=3 exp_name=student_resnet_56_self_distil \
  classifier.depth=56 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  trainer.lr_scheduler.eta_min=0. \
  distill_loader.synth_ratio=0.4 \
  trainer.num_epochs=20 \
  teacher.ckpt_dir="/content/content/content/data/experiments/image_classification/test/preresnet56_cifar100_unitcube/leather_sofa_3"
```
Ensemble distillation with 25k GAN:
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m trial_id=3 exp_name=student_resnet_56_self_distil \
  classifier.depth=56 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  trainer.lr_scheduler.eta_min=0. \
  distill_loader.synth_ratio=0.4 \
  trainer.num_epochs=20 \
  teacher.ckpt_dir="/content/content/content/data/experiments/image_classification/test/preresnet56_cifar100_unitcube"
```
Self distillation with 37.5k GAN:
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m trial_id=3 exp_name=student_resnet_56_self_distil \
  classifier.depth=56 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  trainer.lr_scheduler.eta_min=0. \
  distill_loader.synth_ratio=0.6 \
  trainer.num_epochs=20 \
  teacher.ckpt_dir="/content/content/content/data/experiments/image_classification/test/preresnet56_cifar100_unitcube/leather_sofa_3"
```
Ensemble distillation with 37.5k GAN:
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m trial_id=3 exp_name=student_resnet_56_self_distil \
  classifier.depth=56 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  trainer.lr_scheduler.eta_min=0. \
  distill_loader.synth_ratio=0.6 \
  trainer.num_epochs=20 \
  teacher.ckpt_dir="/content/content/content/data/experiments/image_classification/test/preresnet56_cifar100_unitcube"
```
Self distillation with 50k GAN:
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m trial_id=3 exp_name=student_resnet_56_self_distil \
  classifier.depth=56 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  trainer.lr_scheduler.eta_min=0. \
  distill_loader.synth_ratio=0.8 \
  trainer.num_epochs=20 \
  teacher.ckpt_dir="/content/content/content/data/experiments/image_classification/test/preresnet56_cifar100_unitcube/leather_sofa_3"
```
Ensemble distillation with 50k GAN:
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m trial_id=3 exp_name=student_resnet_56_self_distil \
  classifier.depth=56 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  trainer.lr_scheduler.eta_min=0. \
  distill_loader.synth_ratio=0.8 \
  trainer.num_epochs=20 \
  teacher.ckpt_dir="/content/content/content/data/experiments/image_classification/test/preresnet56_cifar100_unitcube"
```
- For Figure 2:

In downloading MNIST and EMNIST dataset, you might face some HTTP issues, to overcome this, we created a detailed instructions document, you can access it via this link: https://docs.google.com/document/d/13QAJhrD0JMBOcTkTE_uzNtptfxvKLOSEjmIWhiD2VBM/edit?usp=sharing

To train the teacher (only if you want to train all teachers at once, if not, change the trial_id to a single value): 
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m dataset=mnist classifier=lenet \
  dataset.statistics.mean_statistics="[0]" \
  dataset.statistics.std_statistics="[1]" \
  dataset.statistics.max="[1]" \
  dataset.statistics.min="[0]" \
  trainer.optimizer.lr=0.1 \
  trainer.num_epochs=100 trial_id=3,4,5 \
  exp_name=without_distill_mnist_final \
  augmentation.transforms_list=[horizontal_flip,crop] \
  trainer.distill_teacher=False
```
MNIST 60k distillation (without EMNIST, if you have the teacher): 
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m dataset=mnist classifier=lenet \
  dataset.init.num_data=54000 \
  dataset.statistics.mean_statistics="[0]" \
  dataset.statistics.std_statistics="[1]" \
  dataset.statistics.max="[1]" \
  dataset.statistics.min="[0]" \
  trainer.num_epochs=50 trial_id=3 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  teacher.ckpt_dir="/content/content/data/experiments/image_classification/without_distill_mnist_final/threaded_maple_5"
```
Distillation with additional 175k EMNIST: 
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m dataset=emnist classifier=lenet \
  dataset.subsample.ratio=0.25 \
  dataset.statistics.mean_statistics="[0]" \
  dataset.statistics.std_statistics="[1]" \
  dataset.statistics.max="[1]" \
  dataset.statistics.min="[0]" \
  trainer.num_epochs=30 trial_id=3 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  teacher.ckpt_dir="/content/content/data/experiments/image_classification/without_distill_mnist_final/threaded_maple_5"

```
Distillation with additional 350k EMNIST: 
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m dataset=emnist classifier=lenet \
  dataset.subsample.ratio=0.5 \
  dataset.statistics.mean_statistics="[0]" \
  dataset.statistics.std_statistics="[1]" \
  dataset.statistics.max="[1]" \
  dataset.statistics.min="[0]" \
  trainer.num_epochs=30 trial_id=3 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  teacher.ckpt_dir="/content/content/data/experiments/image_classification/without_distill_mnist_final/threaded_maple_5"
```
Distillation with additional 700k EMNIST: 
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m dataset=emnist classifier=lenet \
  dataset.subsample.ratio=1 \
  dataset.statistics.mean_statistics="[0]" \
  dataset.statistics.std_statistics="[1]" \
  dataset.statistics.max="[1]" \
  dataset.statistics.min="[0]" \
  trainer.num_epochs=30 trial_id=3 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  teacher.ckpt_dir="/content/content/data/experiments/image_classification/without_distill_mnist_final/threaded_maple_5"
```
- For Figure 3:


To train and distill the model with Baseline (temperature = 1):
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m trial_id=3 exp_name=student_resnet_56_self_distil \
  classifier.depth=56 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  trainer.lr_scheduler.eta_min=0. \
  trainer.num_epochs=20 \
  distill_loader.temp=1.0 \
  teacher.ckpt_dir="/content/content/content/data/experiments/image_classification/test/preresnet56_cifar100_unitcube"
```
To train and distill the model with Baseline (temperature = 4):
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m trial_id=3 exp_name=student_resnet_56_self_distil \
  classifier.depth=56 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  trainer.lr_scheduler.eta_min=0. \
  trainer.num_epochs=20 \
  distill_loader.temp=4.0 \
  teacher.ckpt_dir="/content/content/content/data/experiments/image_classification/test/preresnet56_cifar100_unitcube"
```
To train and distill the model with Mixup Data Augmentation (temperature = 1):
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m trial_id=3 exp_name=student_resnet_56_self_distil \
  classifier.depth=56 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  trainer.lr_scheduler.eta_min=0. \
  trainer.num_epochs=20 \
  distill_loader.temp=1.0 \
  distill_loader.mixup_alpha=1 \
  teacher.ckpt_dir="/content/content/content/data/experiments/image_classification/test/preresnet56_cifar100_unitcube"
```
To train and distill the model with Rotation (temperature = 1):
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m trial_id=3 exp_name=student_resnet_56_self_distil \
  classifier.depth=56 \
  augmentation.transforms_list=[horizontal_flip,crop,rotation] \
  dataloader.batch_size=128 \
  trainer.lr_scheduler.eta_min=0. \
  trainer.num_epochs=20 \
  distill_loader.temp=1.0 \
  teacher.ckpt_dir="/content/content/content/data/experiments/image_classification/test/preresnet56_cifar100_unitcube"
```
- For Figure 5 (left):

Same as Figure 1

- For Figure 5 (right):

Same as Figure 3

- For Figure 6 (a):


To train and distill the ResNet20 model on SGD Optimization:
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m trial_id=1 exp_name=resnet20-sgd \
  classifier.depth=20 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  trainer.lr_scheduler.eta_min=0. \
  trainer.num_epochs=150
```
To train and distill the ResNet20 model on Adam Optimzer:
```bash
!source gnosis-env/bin/activate && python gnosis/scripts/image_classification.py \
  -m trial_id=3 exp_name=resnet20 \
  classifier.depth=20 \
  augmentation.transforms_list=[horizontal_flip,crop] \
  dataloader.batch_size=128 \
  trainer.lr_scheduler.eta_min=0. \
  trainer.num_epochs=150 \
  trainer.optimizer._target_=torch.optim.Adam \
  trainer.optimizer.lr=5e-4 \
  trainer.optimizer.weight_decay=1e-4 \
  ~trainer.optimizer.momentum \
  ~trainer.optimizer.nesterov
```
