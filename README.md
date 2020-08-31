## An Ensemble of Epoch-wise Empirical Bayes for Few-shot Learning

[![LICENSE](https://img.shields.io/github/license/yaoyao-liu/E3BM?style=flat-square)](https://github.com/yaoyao-liu/E3BM/blob/master/LICENSE)
[![Python](https://img.shields.io/badge/python-3.6-blue.svg?style=flat-square)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/pytorch-1.2.0-%237732a8?style=flat-square)](https://pytorch.org/)
[![CodeFactor](https://img.shields.io/codefactor/grade/github/yaoyao-liu/E3BM/inductive?style=flat-square)](https://www.codefactor.io/repository/github/yaoyao-liu/e3bm)

This repository contains the PyTorch implementation for the Paper "[An Ensemble of Epoch-wise Empirical Bayes for Few-shot Learning](https://arxiv.org/pdf/1904.08479)". If you have any questions on this repository or the related paper, feel free to [create an issue](https://github.com/yaoyao-liu/E3BM/issues/new) or [send me an email](mailto:yaoyao.liu+github@mpi-inf.mpg.de). 

#### Summary

* [Introduction](#introduction)
* [Installation](#installation)
* [Inductive Experiments](#Inductive-Experiments)
* [Transductive Experiments](#transductive-Experiments)
* [Citation](#citation)
* [Acknowledgements](#acknowledgements)

## Introduction

Few-shot learning aims to train efficient predictive models with a few examples. The lack of training data leads to poor models that perform high-variance or low-confidence predictions. In this paper, we propose to meta-learn the ensemble of epoch-wise empirical Bayes models (E<sup>3</sup>BM) to achieve robust predictions. "Epoch-wise" means that each training epoch has a Bayes model whose parameters are specifically learned and deployed. "Empirical" means that the hyperparameters, e.g., used for learning and ensembling the epoch-wise models, are generated by hyperprior learners conditional on task-specific data. We introduce four kinds of hyperprior learners by considering inductive vs. transductive, and epoch-dependent vs. epoch-independent, in the paradigm of meta-learning. We conduct extensive experiments for five-class few-shot tasks on three challenging benchmarks: miniImageNet, tieredImageNet, and FC100, and achieve top performance using the epoch-dependent transductive hyperprior learner, which captures the richest information. Our ablation study shows that both "epoch-wise ensemble" and "empirical" encourage high efficiency and robustness in the model performance.


<p align="center">
    <img src="https://yyliu.net/images/misc/e3bm.png" width="800"/>
</p>

> Figure: Conceptual illustrations of the model adaptation on the blue, red and yellow tasks. (a) MAML is the classical inductive method that meta-learns a network initialization θ that is used to learn a single base-learner on each task. (b) SIB is a transductive method that formulates a variational posterior as a function of both labeled training data T(tr) and unlabeled test data x(te). It also uses a single base-learner and optimizes the learner by running several synthetic gradient steps on x(te). (c) Our E<sup>3</sup>BM is a generic method that learns to combine the epoch-wise base-learners, and to generate task-specific learningcrates α and combination weights v that encourage robust adaptation.

### Installation

In order to run this repository, we advise you to install python 3.6 and PyTorch 1.2.0 with Anaconda.
You may download Anaconda and read the installation instruction on their official website:
<https://www.anaconda.com/download/>

Create a new environment and install PyTorch and torchvision on it:
```bash
conda create --name e3bm-pytorch python=3.6
conda activate e3bm-pytorch
conda install pytorch=1.2.0 
conda install torchvision -c pytorch
```

Install other requirements:
```bash
pip install -r requirements.txt
```

### Inductive Experiments

#### Performance (ResNet-12)

Experiment results (%) for 5-way few-shot classification on ResNet-12 (same as [this repository](https://github.com/kjunelee/MetaOptNet)).


| Method          | Backbone  |𝑚𝑖𝑛𝑖 1-shot   | 𝑚𝑖𝑛𝑖 5-shot  | 𝒕𝒊𝒆𝒓𝒆𝒅 1-shot | 𝒕𝒊𝒆𝒓𝒆𝒅 5-shot |
| --------------  |---------- | ----------  | ----------   |------------ | ------------ |
| [ProtoNet](https://github.com/orobix/Prototypical-Networks-for-Few-shot-Learning-PyTorch)    | ResNet-12 |`60.37 ± 0.83` | `78.02 ± 0.57` | `65.65 ± 0.92` | `83.40 ± 0.65` |
| [MatchNet](https://github.com/gitabcworld/MatchingNetworks)    | ResNet-12 |`63.08 ± 0.80` | `75.99 ± 0.60` | `68.50 ± 0.92` | `80.60 ± 0.71` |
| [MetaOptNet](https://github.com/kjunelee/MetaOptNet)    | ResNet-12 |`62.64 ± 0.61` | `78.63 ± 0.46` | `65.99 ± 0.72` | `81.56 ± 0.53` |
| [Meta-Baseline](https://github.com/cyvius96/few-shot-meta-baseline)    | ResNet-12 |`63.17 ± 0.23` | `79.26 ± 0.17` | `68.62 ± 0.27` | `83.29 ± 0.18` | 
| [CAN](https://github.com/blue-blue272/fewshot-CAN)    | ResNet-12 |`63.85 ± 0.48` | `79.44 ± 0.34` | `69.89 ± 0.51` | `84.93 ± 0.38` | 
| E<sup>3</sup>BM (Ours)   | ResNet-12 |`64.09 ± 0.37` | `80.29 ± 0.25` | `71.34 ± 0.41` | `85.82 ± 0.29` | 

#### Running experiments

Run meta-training with default settings:
```bash
python main.py -backbone resnet12 -shot 1 -way 5 -mode meta_train -dataset miniimagenet
python main.py -backbone resnet12 -shot 5 -way 5 -mode meta_train -dataset miniimagenet
python main.py -backbone resnet12 -shot 1 -way 5 -mode meta_train -dataset tieredimagenet
python main.py -backbone resnet12 -shot 5 -way 5 -mode meta_train -dataset tieredimagenet
```

Run pre-training with default settings:
```bash
python main.py -backbone resnet12 -mode pre_train -dataset miniimagenet
python main.py -backbone resnet12 -mode pre_train -dataset tieredimagenet
```

#### Download resources
All the datasets and pre-trained models will be downloaded automatically.

You may also download the resources on Google Drive using the following links:
<br>
Dataset: [miniImageNet](https://drive.google.com/file/d/1vv3m14kusJcRpCsG-brG_Xk9MnetY9Bt/view?usp=sharing), and [tieredImageNet](https://drive.google.com/file/d/1T-4NVTSa5T6CXKSRbymYLnWp_OrtF-mo/view?usp=sharing)
<br>
Pre-trained models: [Google Drive](https://drive.google.com/file/d/13pzlvn9s4psbZlGpIsYCi9fwQnWeSIkP/view?usp=sharing)
<br>
Meta-trained checkpoints: [Google Drive](https://drive.google.com/drive/folders/17qTMpovfgEV6mRi8M4FkMYLIfBm3smgc?usp=sharing)

### Transductive Experiments

See the transductive setting experiments in this branch: <https://github.com/yaoyao-liu/E3BM/tree/transductive>.



### Citation

Please cite our paper if it is helpful to your work:

```bibtex
@inproceedings{Liu2020E3BM,
  author    = {Liu, Yaoyao and
               Schiele, Bernt and
               Sun, Qianru},
  title     = {An Ensemble of Epoch-wise Empirical Bayes for Few-shot Learning},
  booktitle = {European Conference on Computer Vision (ECCV)},
  year      = {2020}
}
```

### Acknowledgements

Our implementations use the source code from the following repositories:

* [Learning Embedding Adaptation for Few-Shot Learning](https://github.com/Sha-Lab/FEAT)

* [Empirical Bayes Transductive Meta-Learning with Synthetic Gradients](https://github.com/hushell/sib_meta_learn)

* [DeepEMD: Differentiable Earth Mover's Distance for Few-Shot Learning](https://github.com/icoz69/DeepEMD)
