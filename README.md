[![License CC BY-NC-SA 4.0](https://img.shields.io/badge/license-CC4.0-blue.svg)](LICENSE.md)

# HiSD: Image-to-image Translation via Hierarchical Style Disentanglement

Official pytorch implementation of paper "[Image-to-image Translation via Hierarchical Style Disentanglement](https://arxiv.org/abs/2103.01456)".

![fig](assets/fig.png)

## Quick Start

### Clone this repo:

```
git clone https://github.com/imlixinyang/HiSD.git
cd HiSD/
```

### Install the dependencies: (Anaconda is recommended.)
```
conda create -n HiSD python=3.6.6
conda activate HiSD
conda install -y pytorch=1.0.1 torchvision=0.2.2  cudatoolkit=10.1 -c pytorch
pip install pillow tqdm tensorboardx pyyaml
```

### Download the dataset.

We recommend you to download CelebA-HQ from [CelebAMask-HQ](https://github.com/switchablenorms/CelebAMask-HQ).
Anyway you shound get the dataset folder like:
```
celeba_or_celebahq
 - img_dir
   - img0
   - img1
   - ...
 - train_label.txt
```

### Preprocess the dataset.

In our paper, we use fisrt 3000 as test set and remaining 27000 for training.
Carefully check the fisrt few (always two) lines in the label file which is not like the others.
```
python proprecessors/celeba-hq.py --img_path $your_image_path --label_path $your_label_path --target_path datasets --start 3002 --end 30002
```
Then you will get several ".txt" files in the "datasets/", each of them consists of lines of the absolute path of image and its tag-irrelevant conditions (Age and Gender by default).

Almost all custom datasets can be converted into special cases of HiSD.
We provide a script for custom datasets.
You need to organize the folder like:
```
your_training_set
 - Tag0
   - attribute0
     - img0
     - img1
     - ...
   - attribute1
     - ...
 - Tag1
 - ...
```
For example, the AFHQ (one tag and three attributes, remember to split the training and test set first):
```
AFHQ_training
  - cat
    - img0
    - img1
    - ...
  - dog
    - ...
  - wild
    - ...
```

You can Run
```
python proprecessors/custom.py --imgs $your_training_set --target_path datasets/custom.txt
```
For other datasets, please code your preprocessor by yourself.

Here, we provide some links for you to download other available datasets:
- Face
  - **[CelebA](http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html)**
  - **[FFHQ-aging](https://github.com/VEDANTGHODKE/FFHQ-Ageing-Dataset)**
  - **[RaFD](http://www.socsci.ru.nl:8180/RaFD2/RaFD?p=main)**
- Animals
  - **AFHQ** (from [StarGANv2](https://github.com/clovaai/stargan-v2))
  - cat2dog (from [DRIT](https://github.com/HsinYingLee/DRIT))
- Others
  - horse2zebra, orange2apple, summer2winter, monet2photos (from [CycleGAN and Pix2Pix](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix))
  - Selfie2Waifu (from [UGATIT](https://github.com/znxlwm/UGATIT-pytorch))
  - Makeup (from [BeautyGAN](http://liusi-group.com/projects/BeautyGAN))

Dataset in **Bold** means we have tested the generalization of HiSD for this dataset.

### Train.
Following "/core/configs/celeba-hq.yaml" to make the config file fit your machine and dataset.

For a single 1080Ti and CelebA-HQ, you can directly run:
```
python core/train.py --config configs/celeba-hq.yaml --gpus 0
```

The samples and checkpoints are in the "outputs/" dir.
For Celeba-hq dataset, the samples during first 20k iterations will be like:

![training](assets/training.gif)

### Test.

Modify the 'steps' dict in the first few lines in 'core/test.py' and run:
```
python core/test.py --config configs/celeba-hq.yaml --checkpoint $your_checkpoint --input_path $your_input_path --output_path results
```
$your_input_path can be either a image file or a folder of images.
Default 'steps' make every image to be with bangs and glasses using random latent-guided styles.

### Evaluation metrics.

We use [FID](https://github.com/mseitzer/pytorch-fid) for quantitative comparison. For more details, please refer to the paper.

## License

Licensed under the CC BY-NC-SA 4.0 (Attribution-NonCommercial-ShareAlike 4.0 International)

The code is released for academic research use only. For other use, please contact me at [imlixinyang@gmail.com](mailto:imlixinyang@gmail.com).
 
## Citation

If our paper helps your research, please cite it in your publications:
```
@misc{li2021imagetoimage,
      title={Image-to-image Translation via Hierarchical Style Disentanglement}, 
      author={Xinyang Li and Shengchuan Zhang and Jie Hu and Liujuan Cao and Xiaopeng Hong and Xudong Mao and Feiyue Huang and Yongjian Wu and Rongrong Ji},
      year={2021},
      eprint={2103.01456},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}
```

I try my best to make the code easy to understand or further modified because I feel very lucky to start with the clear and readily comprehensible code of [MUNIT](https://github.com/NVlabs/MUNIT) when I'm a beginner.

If you have any problem, please feel free to contact me at [imlixinyang@gmail.com](mailto:imlixinyang@gmail.com) or raise an issue.

## Related Work

- Multi-style/modal: [MUNIT](https://github.com/NVlabs/MUNIT), [DRIT](https://github.com/HsinYingLee/DRIT), [ContentDisentanglement](https://github.com/oripress/ContentDisentanglement), etc.
- Multi-label: [StarGAN](https://github.com/yunjey/stargan), [STGAN](https://github.com/csmliu/STGAN), [RelGAN](https://github.com/elvisyjlin/RelGAN-PyTorch), etc.
- Joint: [SMIT](https://github.com/BCV-Uniandes/SMIT), [SDIT](https://github.com/yaxingwang/SDIT), [DMIT](https://github.com/Xiaoming-Yu/DMIT), [AGUIT](https://github.com/imlixinyang/AGUIT), [ELEGANT](https://github.com/Prinsphield/ELEGANT), [StarGANv2](https://github.com/clovaai/stargan-v2), etc.

