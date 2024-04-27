# High-quality Surface Reconstruction using Gaussian Surfels
Pinxuan Dai*, Jiamin Xu*, Wenxiang Xie, Xinguo Liu, Huamin Wang, Weiwei Xu<sup>†</sup><br>
| [Project](https://turandai.github.io/projects/gaussian_surfels/) | [Paper]() | [arXiv]() | [Data]()<br>

This repository is the official implementation of the SIGGRAPH 24' conference paper "*High-quality Surface Reconstruction using Gaussian Surfels*". The code is mainly build upon the fantastic [3DGS](https://github.com/graphdeco-inria/gaussian-splatting) and borrows the data loading part from [IDR](https://github.com/lioryariv/idr).


## Environment Setup
We did our experiments on Ubuntu 22.04.3, CUDA 11.8, and conda environment on Python 3.7.

Clone this repository:
```shell
git clone https://github.com/turandai/gaussian-surfels
cd gaussian_surfels
```

Create conda environment:
```shell
conda env create --file environment.yml
conda activate gaussian_surfels
```

## Data Preparation
We test our method on subsets of on [DTU](https://roboimagedata.compute.dtu.dk/?page_id=36) and [BlendedMVS](https://github.com/YoYo000/BlendedMVS) datasets. 
We select 15 scenes from DTU and 18 scenes from BlendedMVS and preprocess and normalize the data following [IDR](https://github.com/lioryariv/idr) data convention.
We also adopt [Omnidata](https://github.com/EPFL-VILAB/omnidata) to generate monocular normal prior.
All data needed can be downloaded from [here]().


To test on your own unposed data, we recommend to use [COLMAP](https://github.com/colmap/colmap) for SfM initialization. To estimate monocular normal for your own data, please follow [Omnidata](https://github.com/EPFL-VILAB/omnidata) for additional environment setup, and download the pretrained model:
```shell
cd submodules/omnidata
sh download_surface_normal_models
```
Then run the normal estimation:
```shell
cd submodules/omnidata
python estimate_normal.py --img_path path/to/your/image/directory
```
Note that precomputed normal of forementioned scenes from DTU and BlendedMVS are included in the downloaded dataset, so you don't have to run the normal estimation for them.


## Training
To train a scene:
```shell
python train.py -s path/to/your/data/directory --eval
```
Trained model will be save in ```output/```.
To render images and reconstruct mesh from a trianed model:
```shell
python render.py -m path/to/your/trained/model --img
```

## Evalutation
To evaluate the geometry accuracy on DTU, you have to download the [DTU](https://roboimagedata.compute.dtu.dk/?page_id=36) ground truth point cloud. 
For BlendedMVS evaluation, we fused, denoised and normalized the ground truth multi-view depth maps to a global point cloud as the ground truth geometry, which is included in the our provided dataset above. 
We follow previous work to use [this](https://github.com/jzhangbs/DTUeval-python) code to calculate the Chamfer distance.


<section class="section" id="BibTeX">
  <div class="container is-max-desktop content">
    <h2 class="title">BibTeX</h2>
    <pre><code>@inproceedings{Dai2024GaussianSurfels,
  author    = {Dai, Pinxuan and Xu, Jiamin and Xie, Wenxiang and Liu, Xinguo and Wang, Huamin and Xu, Weiwei},
  title     = {High-quality Surface Reconstruction using Gaussian Surfels},
  publisher = {Association for Computing Machinery},
  booktitle = {SIGGRAPH 2024 Conference Papers},
  year      = {2024},
  doi       = {10.1145/3641519.3657441}
}</code></pre>
  </div>
</section>