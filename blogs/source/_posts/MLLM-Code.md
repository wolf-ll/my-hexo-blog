---
title: MLLM Code
date: 2024-12-10 14:31:13
auther: 小狼
summary: 记录mllm相关代码
categories: 笔记
img: /medias/featureimages/7.jpg
tags:
  - MLLM
  - 代码
---

# MLLM Code

## 数据集

### 编码器训练

#### CC3M

图像caption数据集（共 3318333 张图像），每张图配一个单句字幕。

huggingface**已处理的版本**（[pixparse/cc3m-wds at main](https://huggingface.co/datasets/pixparse/cc3m-wds/tree/main)），使用：

```python
from datasets import load_dataset

ds = load_dataset("pixparse/cc3m-wds")
```

**自行处理的版本**（原始tsv文件：[Conceptual Captions](https://ai.google.com/research/ConceptualCaptions/download)）

先为tsv添加列名`sed -i '1s/^/caption\turl\n/' cc3m.tsv`

然后解析，脚本（[img2dataset/dataset_examples/cc3m.md at main · rom1504/img2dataset](https://github.com/rom1504/img2dataset/blob/main/dataset_examples/cc3m.md)）

```bash
img2dataset --url_list cc3m.tsv --input_format "tsv"\
         --url_col "url" --caption_col "caption" --output_format webdataset\
           --output_folder cc3m --processes_count 16 --thread_count 64 --image_size 256\
             --enable_wandb True
```

caption格式：

```json
{
    "caption": "hard rock artist of hard rock artist performs",
    "url": "https://media.gettyimages.com/photos/lead-singer-steve-perry-of-rock-band-journey-performs-at-the-rosemont-picture-id832243484?s=612x612",
    "key": "000000001",
    "status": "success",
    "error_message": null,
    "width": 415,
    "height": 612,
    "exif": "{\"Image ImageDescription\": \"Lead singer Steve Perry of rock band Journey performs at the Rosemont Horizon in Rosemont, Illinois, June 10, 1983. (Photo by Paul Natkin/Getty Images)\", \"Image Copyright\": \"2017 Paul Natkin\"}",
    "original_width": 415,
    "original_height": 612
}
```

数据集目前放在`Ask-Anything/video_chat2/annotation/videos_images/cc3m`路径下。

#### WebVid-10M

视频caption，缺点是**有水印**。论文使用基本都会做预处理。

CSV文件：

* huggingface：[TempoFunk/webvid-10M · Datasets at Hugging Face](https://huggingface.co/datasets/TempoFunk/webvid-10M)
* HyperAI：[WebVid 大型短视频数据集 / 数据集 / HyperAI超神经 | HyperAI超神经](https://hyper.ai/cn/datasets/17289)

视频数据下载（很多因为网络问题下不下来）：[video2dataset/dataset_examples/WebVid.md at main · iejMac/video2dataset](https://github.com/iejMac/video2dataset/blob/main/dataset_examples/WebVid.md)

```bash
#!/bin/bash

wget -nc http://www.robots.ox.ac.uk/~maxbain/webvid/results_10M_train.csv

video2dataset --url_list="results_10M_train.csv" \
        --input_format="csv" \
        --output-format="webdataset" \
	--output_folder="dataset" \
        --url_col="contentUrl" \
        --caption_col="name" \
        --save_additional_columns='[videoid,page_idx,page_dir,duration]' \
        --enable_wandb=True \
	--config="path/to/config.yaml" \
```

caption格式：

```json
{
    "videoid": 1051729318,
    "page_dir": "095551_095600",
    "duration": "PT00H00M09S",
    "caption": "Mount saint helens, washington - the stunning scenery of a rocky mountains during golden hours - wide shot",
    "url": "https://ak.picdn.net/shutterstock/videos/1051729318/preview/stock-footage-mount-saint-helens-washington-the-stunning-scenery-of-a-rocky-mountains-during-golden-hours.mp4",
    "key": "00000034",
    "status": "success",
    "error_message": null,
    "yt_meta_dict": null
}
```

#### MSR-VTT

MSR-VTT是一个通用的视频字幕数据集，包括10000个视频片段，每个视频剪辑持续约15秒。标准情况下通常使用6153个片段进行训练，497个片段用于验证，2090个片段用于测试。（压缩包6g）

数据集：[CARE/README_DATA.md at master · yangbang18/CARE](https://github.com/yangbang18/CARE/blob/master/README_DATA.md)

下载： [NeurIPS 2022 Spotlight\] Expectation-Maximization Contrastive Learning for Compact Video-and-Language Representations](https://github.com/jpthu17/EMCL?tab=readme-ov-file)（这个版本提供了csv，但标注json是category不是caption，服务器的caption忘了从哪下载的了）

caption格式：

```json
{"video": "video9770.mp4", "caption": "a person is connecting something to system", "duration": 10.8}, {"video": "video9771.mp4", "caption": "a little girl does gymnastics", "duration": 13.78}
```

### 连接器训练

#### COCO

- COCO数据集包含20万个图像；
- 80个类别中有超过50万个目标标注,它是最广泛公开的目标检测数据库；

下载：[COCO数据集的下载、介绍及如何使用（数据载入及数据增广，含代码）_coco数据集下载-CSDN博客](https://blog.csdn.net/qq_41847324/article/details/86224628)

标注数据格式：

```json
{"info": {"description": "COCO 2017 Dataset","url": "http://cocodataset.org","version": "1.0","year": 2017,"contributor": "COCO Consortium","date_created": "2017/09/01"},"licenses": [{"url": "http://creativecommons.org/licenses/by-nc-sa/2.0/","id": 1,"name": "Attribution-NonCommercial-ShareAlike License"},{"url": "http://creativecommons.org/licenses/by-nc/2.0/","id": 2,"name": "Attribution-NonCommercial License"},…………],
 "images": [{"license": 3,"file_name": "000000391895.jpg","coco_url": "http://images.cocodataset.org/train2017/000000391895.jpg","height": 360,"width": 640,"date_captured": "2013-11-14 11:18:45","flickr_url": "http://farm9.staticflickr.com/8186/8119368305_4e622c8349_z.jpg","id": 391895},{"license": 4,"file_name": "000000522418.jpg","coco_url": "http://images.cocodataset.org/train2017/000000522418.jpg","height": 480,"width": 640,"date_captured": "2013-11-14 11:38:44","flickr_url": "http://farm1.staticflickr.com/1/127244861_ab0c0381e7_z.jpg","id": 522418},…………],
 "annotations": [{"image_id": 203564,"id": 37,"caption": "A bicycle replica with a clock as the front wheel."},{"image_id": 322141,"id": 49,"caption": "A room with blue walls and a white sink and door."},{"image_id": 16977,"id": 89,"caption": "A car that seems to be parked illegally behind a legally parked car"},…………]
```



## InternLM-XComposer2-4KHD

IXC2-4KHD结构

* 视觉编码器：OpenAI CLIP ViT-L-14-336，添加动态分辨率方案
* projector 层：MLP
* LLM ：为最新的 InternLM2
* 预训练：训练Vit，MLP投影和LLM；
* 微调：Partial LoRA (P-LoRA)

<img src="MLLM-Code\image-20241211142115207.png" alt="image-20241211142115207" style="zoom: 67%;" />

和 llava next 一样，模型输入包括两个视图，具体做法为：

- 对于给定的任意一张图片，训练时候需要指定这张图片希望切分为最多为多少个 patch，例如论文说的 HD-9，HD-25 和 HD-55，HD-9 也就是说经过处理后这张图片，这种图片最多(**可以少**)可以切分为 9 个 patch，假设正好可以切分为 9 个 patch，那么可能输出分辨率为 1008×1008, 672×1344, 336×3024，至于到底选择的是那个分辨率则是依据图片宽高比来确定的，算法的目标是**在接近 9 个 patch 情况下，图片后续 padding 的像素最少即可**
- 假设算出来是 672×1344，那么图片会在先进行保持宽高比情况下 resize，然后 padding 为 672×1344（下面函数到这里为止）
- 上述就得到高分辨率图了，将这个 672×1344 图片直接强制 resize 为 336x336 就得到了**全局视图**
- 为了让模型更容易理解 patch，如上所示，作者在每一行后面加一个可学习的 \n 特殊 token，并且也加了一个可学习的 sp 特殊 token
- 由于在 HD 输入下，token 非常多，直接训练肯定会炸了，例如 HD-55，此时的 patch 一共 55+1=56 个，每个 patch 输出 576 个 token，那么就有 32256 个 image token，这就有点夸张了。所以作者简单的进行 token merge了，就是相邻的 4 个 token 合并，将信息转换到通道维度了，**此时最多的 Image token 是 8064 了，虽然也很多，但是至少看起来还行。**

```python
def HD_transform(img, hd_num=9):	# hd_num 期望切分的最大patch数
    width, height = img.size
    if width < height:
        width, height = img.size
    ratio = (width / height)
    print(ratio)

    # 假设某条边扩展为 n 个 patch，此时可以得到在宽高比不变情况下另一条边最多可以扩展为 n/ratio 个 patch
    # 如果总的 patch 数 (scale*np.ceil(scale/ratio) )接近 hd_num 但是不超过 hd_num，那么就选择这个
    scale = 1  # scale计算某条边扩展到多少patch数
    while scale*np.ceil(scale/ratio) <= hd_num:
        scale += 1
    scale -= 1
    
    new_w = int(scale * 336)	# 每个patch 336*336
    new_h = int(new_w / ratio)

    img = transforms.functional.resize(img, [new_h, new_w],)
    # 确保能够被 336 整除，周围padding填充
    img = padding_336(img)
    return img
```

## VideoChat2



## 配置记录

### pycharm映射

<img src="MLLM-Code\image-20250103155952566.png" alt="image-20250103155952566" style="zoom: 50%;" /><img src="MLLM-Code\image-20250103160038049.png" alt="image-20250103160038049" style="zoom: 67%;" />

### 依赖版本

cuda-cudart               12.1.105                      0    nvidia
cuda-cupti                12.1.105                      0    nvidia
cuda-libraries            12.1.0                        0    nvidia
cuda-nvrtc                12.1.105                      0    nvidia
cuda-nvtx                 12.1.105                      0    nvidia
cuda-opencl               12.4.127             h6a678d5_0    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
**cuda-runtime              12.1.0                        0    nvidia**
**cuda-version              12.4                 hbda6634_3    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main**
dav1d                     1.2.1                h5eee18b_0    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
decord                    0.6.0                    pypi_0    pypi
deepspeed                 0.14.2                   pypi_0    pypi
**ffmpeg                    6.1.1                h4c62175_0    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main**
**ffmpeg-python             0.2.0                    pypi_0    pypi**
filelock                  3.13.1           py39h06a4308_0    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
fire                      0.5.0                    pypi_0    pypi
flatbuffers               24.3.25                  pypi_0    pypi
**fontconfig                2.14.1               h4c34cd2_2    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main**
**freetype                  2.12.1               h4a9f257_0    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main**
fsspec                    2024.10.0                pypi_0    pypi
future                    1.0.0                    pypi_0    pypi
markdown                  3.7                      pypi_0    pypi
markdown-it-py            3.0.0                    pypi_0    pypi

**numpy                     1.23.5           py39hf6e8229_1    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main**
**numpy-base                1.23.5           py39h060ed82_1    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main**
nvidia-cublas-cu12        12.4.5.8                 pypi_0    pypi

**pandas                    2.0.3            py39h1128e8f_0    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main**
**peft                      0.4.0                    pypi_0    pypi**
**pillow                    10.0.0                   pypi_0    pypi**
**pip                       24.2             py39h06a4308_0    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main**
pixman                    0.40.0               h7f8727e_1    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
plaster                   1.1.2                    pypi_0    pypi
plaster-pastedeploy       1.0.1                    pypi_0    pypi
**python                    3.9.20               he870216_1    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main**
python-dateutil           2.9.0post0       py39h06a4308_2    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
python-tzdata             2023.3             pyhd3eb1b0_0    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
python3-openid            3.2.0                    pypi_0    pypi
python_abi                3.9                      2_cp39    http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge
**pytorch-cuda              12.1                 ha16c6d3_6    pytorch**
pytorch-mutex             1.0                        cuda    pytorch

**scipy                     1.13.0                   pypi_0    pypi**
**sentencepiece             0.1.99                   pypi_0    pypi**
sqlalchemy                2.0.36                   pypi_0    pypi
sqlite                    3.45.3               h5eee18b_0    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
tensorboard-data-server   0.7.2                    pypi_0    pypi
tensorflow                2.16.1                   pypi_0    pypi
tensorflow-io-gcs-filesystem 0.37.1                   pypi_0    pypi
**tokenizers                0.15.2                   pypi_0    pypi**
**torch                     1.13.1+cu117             pypi_0    pypi**
**torchaudio                0.13.1+cu117             pypi_0    pypi**
**torchvision               0.14.1+cu117             pypi_0    pypi**
**tqdm                      4.66.1             pyhd8ed1ab_0    http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge**
transaction               5.0                      pypi_0    pypi
**transformers              4.37.1                   pypi_0    pypi**
translationstring         1.4                      pypi_0    pypi
**urllib3                   2.2.3            py39h06a4308_0    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main**
velruse                   1.1.1                    pypi_0    pypi
venusian                  3.1.1                    pypi_0    pypi
**wandb                     0.16.6             pyhd8ed1ab_1    http://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge**
webdataset                0.2.100                  pypi_0    pypi
webob                     1.8.9                    pypi_0    pypi

**wheel                     0.44.0           py39h06a4308_0    http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main**
wrapt                     1.17.0                   pypi_0    pypi

## 参考

CC3M数据集：[pixparse/cc3m-wds at main](https://huggingface.co/datasets/pixparse/cc3m-wds/tree/main)

WebVid数据集：[TempoFunk/webvid-10M · Datasets at Hugging Face](https://huggingface.co/datasets/TempoFunk/webvid-10M)

MSR-VTT数据集：[准备 MSR-VTT 检索/视频问答数据集 — MMAction2 1.2.0 文档](https://mmaction2.readthedocs.io/zh-cn/dev-1.x/dataset_zoo/msrvtt.html)

* 下载：[NeurIPS 2022 Spotlight\] Expectation-Maximization Contrastive Learning for Compact Video-and-Language Representations](https://github.com/jpthu17/EMCL?tab=readme-ov-file)

* 对应google云：[MSRVTT - Google Drive](https://drive.google.com/drive/folders/1LYVUCPRxpKMRjCSfB_Gz-ugQa88FqDu_)
* 这个版本不是服务器用的版本，标注json只有category没有caption

COCO数据集：[Dataset之COCO数据集：COCO数据集的简介、下载、使用方法之详细攻略-CSDN博客](https://blog.csdn.net/qq_41185868/article/details/82939959)

Video-LLaVA训练：[Video-LLaVA/TRAIN_AND_VALIDATE.md at main · PKU-YuanGroup/Video-LLaVA](https://github.com/PKU-YuanGroup/Video-LLaVA/blob/main/TRAIN_AND_VALIDATE.md)

IXC2-4KHD：[MLLM-算法推荐-2024.4.12\] InternLM-XComposer2-4KHD 处理高分辨率有一手 - 知乎](https://zhuanlan.zhihu.com/p/692049805)

pycharm连接服务器（核心是deployment ssh配置+mapping填对）：[Pycharm远程连接服务器进行代码的运行与调试_remote sdk is saved in idesetting-CSDN博客](https://blog.csdn.net/qq_42730750/article/details/119249193)