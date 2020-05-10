---
layout: post
title:    输出 9*9 乘法口诀表
categories: python
tags: python
author: livingbody

---



# PaddleHub之《青春有你2》作业：五人识别

## 一、任务简介

图像分类是计算机视觉的重要领域，它的目标是将图像分类到预定义的标签。近期，许多研究者提出很多不同种类的神经网络，并且极大的提升了分类算法的性能。本文以自己创建的数据集：青春有你2中选手识别为例子，介绍如何使用PaddleHub进行图像分类任务。

<div  align="center">   
<img src="https://ai-studio-static-online.cdn.bcebos.com/dbaf5ba718f749f0836c342fa67f6d7954cb89f3796b4c8b981a03a1635f2fbe" width = "350" height = "250" align=center />
</div>



```python
#CPU环境启动请务必执行该指令
%set_env CPU_NUM=1 
```

    env: CPU_NUM=1



```python
#安装paddlehub
!pip install paddlehub==1.6.0 -i https://pypi.tuna.tsinghua.edu.cn/simple
```

    Looking in indexes: https://pypi.tuna.tsinghua.edu.cn/simple
    Collecting paddlehub==1.6.0
    [?25l  Downloading https://pypi.tuna.tsinghua.edu.cn/packages/7f/9f/6617c2b8e9c5d847803ae89924b58bccd1b8fb2c98aa00e16531540591f2/paddlehub-1.6.0-py3-none-any.whl (206kB)
    [K     |████████████████████████████████| 215kB 10.0MB/s eta 0:00:01
    [?25hRequirement already satisfied: protobuf>=3.6.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (3.10.0)
    Requirement already satisfied: pandas; python_version >= "3" in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (0.23.4)
    Requirement already satisfied: requests in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (2.22.0)
    Requirement already satisfied: sentencepiece in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (0.1.85)
    Requirement already satisfied: chardet==3.0.4 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (3.0.4)
    Requirement already satisfied: gunicorn>=19.10.0; sys_platform != "win32" in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (20.0.4)
    Requirement already satisfied: six>=1.10.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (1.12.0)
    Requirement already satisfied: numpy; python_version >= "3" in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (1.16.4)
    Requirement already satisfied: yapf==0.26.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (0.26.0)
    Requirement already satisfied: tensorboard>=1.15 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (2.1.0)
    Requirement already satisfied: flask>=1.1.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (1.1.1)
    Requirement already satisfied: flake8 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (3.7.9)
    Requirement already satisfied: tb-paddle in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (0.3.6)
    Requirement already satisfied: colorlog in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (4.1.0)
    Requirement already satisfied: Pillow in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (6.2.0)
    Requirement already satisfied: nltk in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (3.4.5)
    Requirement already satisfied: cma==2.7.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (2.7.0)
    Requirement already satisfied: opencv-python in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (4.1.1.26)
    Requirement already satisfied: pre-commit in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (1.21.0)
    Requirement already satisfied: pyyaml in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from paddlehub==1.6.0) (5.1.2)
    Requirement already satisfied: setuptools in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from protobuf>=3.6.0->paddlehub==1.6.0) (41.4.0)
    Requirement already satisfied: pytz>=2011k in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pandas; python_version >= "3"->paddlehub==1.6.0) (2019.3)
    Requirement already satisfied: python-dateutil>=2.5.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pandas; python_version >= "3"->paddlehub==1.6.0) (2.8.0)
    Requirement already satisfied: certifi>=2017.4.17 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from requests->paddlehub==1.6.0) (2019.9.11)
    Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from requests->paddlehub==1.6.0) (1.25.6)
    Requirement already satisfied: idna<2.9,>=2.5 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from requests->paddlehub==1.6.0) (2.8)
    Requirement already satisfied: grpcio>=1.24.3 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from tensorboard>=1.15->paddlehub==1.6.0) (1.26.0)
    Requirement already satisfied: google-auth<2,>=1.6.3 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from tensorboard>=1.15->paddlehub==1.6.0) (1.10.0)
    Requirement already satisfied: wheel>=0.26; python_version >= "3" in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from tensorboard>=1.15->paddlehub==1.6.0) (0.33.6)
    Requirement already satisfied: google-auth-oauthlib<0.5,>=0.4.1 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from tensorboard>=1.15->paddlehub==1.6.0) (0.4.1)
    Requirement already satisfied: werkzeug>=0.11.15 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from tensorboard>=1.15->paddlehub==1.6.0) (0.16.0)
    Requirement already satisfied: markdown>=2.6.8 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from tensorboard>=1.15->paddlehub==1.6.0) (3.1.1)
    Requirement already satisfied: absl-py>=0.4 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from tensorboard>=1.15->paddlehub==1.6.0) (0.8.1)
    Requirement already satisfied: Jinja2>=2.10.1 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from flask>=1.1.0->paddlehub==1.6.0) (2.10.1)
    Requirement already satisfied: click>=5.1 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from flask>=1.1.0->paddlehub==1.6.0) (7.0)
    Requirement already satisfied: itsdangerous>=0.24 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from flask>=1.1.0->paddlehub==1.6.0) (1.1.0)
    Requirement already satisfied: pycodestyle<2.6.0,>=2.5.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from flake8->paddlehub==1.6.0) (2.5.0)
    Requirement already satisfied: mccabe<0.7.0,>=0.6.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from flake8->paddlehub==1.6.0) (0.6.1)
    Requirement already satisfied: pyflakes<2.2.0,>=2.1.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from flake8->paddlehub==1.6.0) (2.1.1)
    Requirement already satisfied: entrypoints<0.4.0,>=0.3.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from flake8->paddlehub==1.6.0) (0.3)
    Requirement already satisfied: moviepy in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from tb-paddle->paddlehub==1.6.0) (1.0.1)
    Requirement already satisfied: identify>=1.0.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pre-commit->paddlehub==1.6.0) (1.4.10)
    Requirement already satisfied: importlib-metadata; python_version < "3.8" in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pre-commit->paddlehub==1.6.0) (0.23)
    Requirement already satisfied: toml in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pre-commit->paddlehub==1.6.0) (0.10.0)
    Requirement already satisfied: aspy.yaml in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pre-commit->paddlehub==1.6.0) (1.3.0)
    Requirement already satisfied: cfgv>=2.0.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pre-commit->paddlehub==1.6.0) (2.0.1)
    Requirement already satisfied: nodeenv>=0.11.1 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pre-commit->paddlehub==1.6.0) (1.3.4)
    Requirement already satisfied: virtualenv>=15.2 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pre-commit->paddlehub==1.6.0) (16.7.9)
    Requirement already satisfied: pyasn1-modules>=0.2.1 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from google-auth<2,>=1.6.3->tensorboard>=1.15->paddlehub==1.6.0) (0.2.7)
    Requirement already satisfied: rsa<4.1,>=3.1.4 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from google-auth<2,>=1.6.3->tensorboard>=1.15->paddlehub==1.6.0) (4.0)
    Requirement already satisfied: cachetools<5.0,>=2.0.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from google-auth<2,>=1.6.3->tensorboard>=1.15->paddlehub==1.6.0) (4.0.0)
    Requirement already satisfied: requests-oauthlib>=0.7.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from google-auth-oauthlib<0.5,>=0.4.1->tensorboard>=1.15->paddlehub==1.6.0) (1.3.0)
    Requirement already satisfied: MarkupSafe>=0.23 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from Jinja2>=2.10.1->flask>=1.1.0->paddlehub==1.6.0) (1.1.1)
    Requirement already satisfied: imageio-ffmpeg>=0.2.0; python_version >= "3.4" in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from moviepy->tb-paddle->paddlehub==1.6.0) (0.3.0)
    Requirement already satisfied: proglog<=1.0.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from moviepy->tb-paddle->paddlehub==1.6.0) (0.1.9)
    Requirement already satisfied: decorator<5.0,>=4.0.2 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from moviepy->tb-paddle->paddlehub==1.6.0) (4.4.0)
    Requirement already satisfied: imageio<3.0,>=2.5; python_version >= "3.4" in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from moviepy->tb-paddle->paddlehub==1.6.0) (2.6.1)
    Requirement already satisfied: tqdm<5.0,>=4.11.2 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from moviepy->tb-paddle->paddlehub==1.6.0) (4.36.1)
    Requirement already satisfied: zipp>=0.5 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from importlib-metadata; python_version < "3.8"->pre-commit->paddlehub==1.6.0) (0.6.0)
    Requirement already satisfied: pyasn1<0.5.0,>=0.4.6 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from pyasn1-modules>=0.2.1->google-auth<2,>=1.6.3->tensorboard>=1.15->paddlehub==1.6.0) (0.4.8)
    Requirement already satisfied: oauthlib>=3.0.0 in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from requests-oauthlib>=0.7.0->google-auth-oauthlib<0.5,>=0.4.1->tensorboard>=1.15->paddlehub==1.6.0) (3.1.0)
    Requirement already satisfied: more-itertools in /opt/conda/envs/python35-paddle120-env/lib/python3.7/site-packages (from zipp>=0.5->importlib-metadata; python_version < "3.8"->pre-commit->paddlehub==1.6.0) (7.2.0)
    Installing collected packages: paddlehub
      Found existing installation: paddlehub 1.5.0
        Uninstalling paddlehub-1.5.0:
          Successfully uninstalled paddlehub-1.5.0
    Successfully installed paddlehub-1.6.0


## 二、任务实践
### Step1、基础工作

加载数据文件

导入python包


```python
!unzip -o file.zip -d ./dataset/
```

    unzip:  cannot find or open file.zip, file.zip.zip or file.zip.ZIP.



```python

```


```python
!unzip ./data/train.zip -d ./dataset/train
!ls
```

    Archive:  ./data/train.zip
    replace ./dataset/train/anqi/anqi0.jpg? [y]es, [n]o, [A]ll, [N]one, [r]ename: 


```python
import paddlehub as hub
```

### Step2、加载预训练模型

接下来我们要在PaddleHub中选择合适的预训练模型来Finetune，由于是图像分类任务，因此我们使用经典的ResNet-50作为预训练模型。PaddleHub提供了丰富的图像分类预训练模型，包括了最新的神经网络架构搜索类的PNASNet，我们推荐您尝试不同的预训练模型来获得更好的性能。


```python
import os

def generate_train_tlist():
   # 待搜索的目录路径
    result=[]
    path = "dataset/train"
    # 待搜索的名称
    stars = {'yushuxin': 0, 'xujiaqi': 1, 'zhaoxiaotang': 2, 'anqi': 3, r'wangchengxuan': 4}
    for root, dirs, files in os.walk(path):
        for f in files:
            ff = os.path.join(root, f)
            # print(ff)
            fff='t'+ff.strip('dataset/')
            # print(fff)
            # print(f)
            name=f[:-5]
            # print(name)
            # print(' %s %d'% ( fff, stars[name]))
            result.append('%s %d'% ( fff, stars[name]))
    return result
train_list=generate_train_tlist()
with open("./dataset/train_list.txt", "w") as f:
    for line in train_list:
        print(line)
        f.writelines(line)
        f.writelines("\n")


```

    train/anqi/anqi4.jpg 3
    train/anqi/anqi8.jpg 3
    train/anqi/anqi7.jpg 3
    train/anqi/anqi3.jpg 3
    train/anqi/anqi0.jpg 3
    train/anqi/anqi5.jpg 3
    train/anqi/anqi2.jpg 3
    train/anqi/anqi6.jpg 3
    train/anqi/anqi9.jpg 3
    train/anqi/anqi1.jpg 3
    train/wangchengxuan/wangchengxuan5.jpg 4
    train/wangchengxuan/wangchengxuan7.jpg 4
    train/wangchengxuan/wangchengxuan4.jpg 4
    train/wangchengxuan/wangchengxuan2.jpg 4
    train/wangchengxuan/wangchengxuan9.jpg 4
    train/wangchengxuan/wangchengxuan8.jpg 4
    train/wangchengxuan/wangchengxuan3.jpg 4
    train/wangchengxuan/wangchengxuan0.jpg 4
    train/wangchengxuan/wangchengxuan6.jpg 4
    train/wangchengxuan/wangchengxuan1.jpg 4
    train/yushuxin/yushuxin0.jpg 0
    train/yushuxin/yushuxin9.jpg 0
    train/yushuxin/yushuxin5.jpg 0
    train/yushuxin/yushuxin4.jpg 0
    train/yushuxin/yushuxin3.jpg 0
    train/yushuxin/yushuxin2.jpg 0
    train/yushuxin/yushuxin7.jpg 0
    train/yushuxin/yushuxin6.jpg 0
    train/yushuxin/yushuxin8.jpg 0
    train/yushuxin/yushuxin1.jpg 0
    train/xujiaqi/xujiaqi4.jpg 1
    train/xujiaqi/xujiaqi2.jpg 1
    train/xujiaqi/xujiaqi6.jpg 1
    train/xujiaqi/xujiaqi1.jpg 1
    train/xujiaqi/xujiaqi9.jpg 1
    train/xujiaqi/xujiaqi8.jpg 1
    train/xujiaqi/xujiaqi5.jpg 1
    train/xujiaqi/xujiaqi3.jpg 1
    train/xujiaqi/xujiaqi0.jpg 1
    train/xujiaqi/xujiaqi7.jpg 1
    train/zhaoxiaotang/zhaoxiaotang7.jpg 2
    train/zhaoxiaotang/zhaoxiaotang6.jpg 2
    train/zhaoxiaotang/zhaoxiaotang1.jpg 2
    train/zhaoxiaotang/zhaoxiaotang9.jpg 2
    train/zhaoxiaotang/zhaoxiaotang2.jpg 2
    train/zhaoxiaotang/zhaoxiaotang0.jpg 2
    train/zhaoxiaotang/zhaoxiaotang4.jpg 2
    train/zhaoxiaotang/zhaoxiaotang5.jpg 2
    train/zhaoxiaotang/zhaoxiaotang8.jpg 2
    train/zhaoxiaotang/zhaoxiaotang3.jpg 2



```python
!hub install ernie
```

    Module ernie already installed in /home/aistudio/.paddlehub/modules/ernie



```python
module = hub.Module(name="resnet_v2_50_imagenet")
```

    [32m[2020-04-26 13:03:09,478] [    INFO] - Installing resnet_v2_50_imagenet module[0m
    [32m[2020-04-26 13:03:09,498] [    INFO] - Module resnet_v2_50_imagenet already installed in /home/aistudio/.paddlehub/modules/resnet_v2_50_imagenet[0m


### Step3、数据准备

接着需要加载图片数据集。我们使用自定义的数据进行体验，请查看[适配自定义数据](https://github.com/PaddlePaddle/PaddleHub/wiki/PaddleHub适配自定义数据完成FineTune)


```python
from paddlehub.dataset.base_cv_dataset import BaseCVDataset
   
class DemoDataset(BaseCVDataset):	
   def __init__(self):	
       # 数据集存放位置
       
       self.dataset_dir = "dataset"
       super(DemoDataset, self).__init__(
           base_path=self.dataset_dir,
           train_list_file="train_list.txt",
        #    validate_list_file="validate_list.txt",
           test_list_file="test_list.txt",
           label_list_file="label_list.txt",
           )
dataset = DemoDataset()
```

### Step4、生成数据读取器

接着生成一个图像分类的reader，reader负责将dataset的数据进行预处理，接着以特定格式组织并输入给模型进行训练。

当我们生成一个图像分类的reader时，需要指定输入图片的大小


```python
data_reader = hub.reader.ImageClassificationReader(
    image_width=module.get_expected_image_width(),
    image_height=module.get_expected_image_height(),
    images_mean=module.get_pretrained_images_mean(),
    images_std=module.get_pretrained_images_std(),
    dataset=dataset)
```

    [32m[2020-04-26 13:07:58,826] [    INFO] - Dataset label map = {'虞书欣': 0, '许佳琪': 1, '赵小棠': 2, '安崎': 3, '王承渲': 4}[0m


### Step5、配置策略
在进行Finetune前，我们可以设置一些运行时的配置，例如如下代码中的配置，表示：

* `use_cuda`：设置为False表示使用CPU进行训练。如果您本机支持GPU，且安装的是GPU版本的PaddlePaddle，我们建议您将这个选项设置为True；

* `epoch`：迭代轮数；

* `batch_size`：每次训练的时候，给模型输入的每批数据大小为32，模型训练时能够并行处理批数据，因此batch_size越大，训练的效率越高，但是同时带来了内存的负荷，过大的batch_size可能导致内存不足而无法训练，因此选择一个合适的batch_size是很重要的一步；

* `log_interval`：每隔10 step打印一次训练日志；

* `eval_interval`：每隔50 step在验证集上进行一次性能评估；

* `checkpoint_dir`：将训练的参数和数据保存到cv_finetune_turtorial_demo目录中；

* `strategy`：使用DefaultFinetuneStrategy策略进行finetune；

更多运行配置，请查看[RunConfig](https://github.com/PaddlePaddle/PaddleHub/wiki/PaddleHub-API:-RunConfig)

同时PaddleHub提供了许多优化策略，如`AdamWeightDecayStrategy`、`ULMFiTStrategy`、`DefaultFinetuneStrategy`等，详细信息参见[策略](https://github.com/PaddlePaddle/PaddleHub/wiki/PaddleHub-API:-Strategy)


```python
config = hub.RunConfig(
    use_cuda=True,                              #是否使用GPU训练，默认为False；
    num_epoch=3,                                #Fine-tune的轮数；
    checkpoint_dir="cv_finetune_turtorial_demo" ,#模型checkpoint保存路径, 若用户没有指定，程序会自动生成；
    batch_size=3,                              #训练的批大小，如果使用GPU，请根据实际情况调整batch_size；
    # eval_interval=3,                           #模型评估的间隔，默认每100个step评估一次验证集；
    log_interval=10,
    strategy=hub.finetune.strategy.DefaultFinetuneStrategy())  #Fine-tune优化策略；
```

    [32m[2020-04-26 13:06:01,681] [    INFO] - Checkpoint dir: cv_finetune_turtorial_demo[0m


### Step6、组建Finetune Task
有了合适的预训练模型和准备要迁移的数据集后，我们开始组建一个Task。

由于该数据设置是一个二分类的任务，而我们下载的分类module是在ImageNet数据集上训练的千分类模型，所以我们需要对模型进行简单的微调，把模型改造为一个二分类模型：

1. 获取module的上下文环境，包括输入和输出的变量，以及Paddle Program；
2. 从输出变量中找到特征图提取层feature_map；
3. 在feature_map后面接入一个全连接层，生成Task；


```python
input_dict, output_dict, program = module.context(trainable=True)
img = input_dict["image"]
feature_map = output_dict["feature_map"]
feed_list = [img.name]

task = hub.ImageClassifierTask(
    data_reader=data_reader,
    feed_list=feed_list,
    feature=feature_map,
    num_classes=dataset.num_labels,
    config=config)
```

    [32m[2020-04-26 13:06:04,337] [    INFO] - 267 pretrained paramaters loaded by PaddleHub[0m


### Step5、开始Finetune

我们选择`finetune_and_eval`接口来进行模型训练，这个接口在finetune的过程中，会周期性的进行模型效果的评估，以便我们了解整个训练过程的性能变化。


```python
run_states = task.finetune_and_eval()
```

    [32m[2020-04-26 13:06:10,841] [    INFO] - Strategy with slanted triangle learning rate, L2 regularization, [0m
    [32m[2020-04-26 13:06:10,873] [    INFO] - Try loading checkpoint from cv_finetune_turtorial_demo/ckpt.meta[0m
    [32m[2020-04-26 13:06:10,874] [    INFO] - PaddleHub model checkpoint not found, start from scratch...[0m
    [32m[2020-04-26 13:06:10,909] [    INFO] - PaddleHub finetune start[0m
    [36m[2020-04-26 13:06:12,540] [   TRAIN] - step 10 / 50: loss=0.89901 acc=0.73333 [step/sec: 7.16][0m
    [36m[2020-04-26 13:06:13,915] [   TRAIN] - step 20 / 50: loss=0.38457 acc=1.00000 [step/sec: 10.22][0m
    [36m[2020-04-26 13:06:15,447] [   TRAIN] - step 30 / 50: loss=0.11394 acc=1.00000 [step/sec: 6.95][0m
    [36m[2020-04-26 13:06:16,902] [   TRAIN] - step 40 / 50: loss=0.06314 acc=1.00000 [step/sec: 7.48][0m
    [36m[2020-04-26 13:06:18,353] [   TRAIN] - step 50 / 50: loss=0.04763 acc=1.00000 [step/sec: 7.62][0m
    [32m[2020-04-26 13:06:18,432] [    INFO] - Load the best model from cv_finetune_turtorial_demo/best_model[0m
    [32m[2020-04-26 13:06:18,433] [    INFO] - Evaluation on test dataset start[0m
    share_vars_from is set, scope is ignored.
    [34m[2020-04-26 13:06:19,083] [    EVAL] - [test dataset evaluation result] loss=0.00011 acc=1.00000 [step/sec: 19.32][0m
    [32m[2020-04-26 13:06:19,085] [    INFO] - Saving model checkpoint to cv_finetune_turtorial_demo/step_51[0m
    [32m[2020-04-26 13:06:20,090] [    INFO] - PaddleHub finetune finished.[0m


### Step6、预测

当Finetune完成后，我们使用模型来进行预测，先通过以下命令来获取测试的图片


```python
import numpy as np
import matplotlib.pyplot as plt 
import matplotlib.image as mpimg

with open("dataset/test_list.txt","r") as f:
    filepath = f.readlines()
    # print(filepath)

data = [filepath[0].split(" ")[0],filepath[1].split(" ")[0],filepath[2].split(" ")[0],filepath[3].split(" ")[0],filepath[4].split(" ")[0]]
print(data)
label_map = dataset.label_dict()
index = 0
run_states = task.predict(data=data)
results = [run_state.run_results for run_state in run_states]
print(results)
print(50*'*')
for batch_result in results:
    print(batch_result)
    print(50*'*')
    batch_result = np.argmax(batch_result, axis=2)[0]
    print(batch_result)
    for result in batch_result:
        index += 1
        result = label_map[result]
        print("input %i is %s, and the predict result is %s" %
              (index, data[index - 1], result))

```

    [32m[2020-04-26 13:08:42,487] [    INFO] - PaddleHub predict start[0m
    [32m[2020-04-26 13:08:42,487] [    INFO] - Load the best model from cv_finetune_turtorial_demo/best_model[0m


    ['dataset/test/yushuxin.jpg', 'dataset/test/xujiaqi.jpg', 'dataset/test/zhaoxiaotang.jpg', 'dataset/test/anqi.jpg', 'dataset/test/wangchengxuan.jpg']
    [[array([[9.99820173e-01, 7.76551133e-06, 3.23944623e-05, 1.07691012e-04,
            3.19731771e-05],
           [2.23369602e-06, 9.99973655e-01, 9.16190515e-07, 2.26883185e-05,
            5.21339530e-07],
           [2.94848701e-06, 1.81872983e-05, 9.99861002e-01, 8.90642877e-06,
            1.08885535e-04]], dtype=float32)], [array([[2.0881200e-06, 4.9753922e-05, 5.9350541e-06, 9.9994111e-01,
            1.0736142e-06],
           [3.4572211e-05, 3.4797737e-05, 3.2656546e-05, 3.2816235e-05,
            9.9986517e-01]], dtype=float32)]]
    **************************************************
    [array([[9.99820173e-01, 7.76551133e-06, 3.23944623e-05, 1.07691012e-04,
            3.19731771e-05],
           [2.23369602e-06, 9.99973655e-01, 9.16190515e-07, 2.26883185e-05,
            5.21339530e-07],
           [2.94848701e-06, 1.81872983e-05, 9.99861002e-01, 8.90642877e-06,
            1.08885535e-04]], dtype=float32)]
    **************************************************
    [0 1 2]
    input 1 is dataset/test/yushuxin.jpg, and the predict result is 虞书欣
    input 2 is dataset/test/xujiaqi.jpg, and the predict result is 许佳琪
    input 3 is dataset/test/zhaoxiaotang.jpg, and the predict result is 赵小棠
    [array([[2.0881200e-06, 4.9753922e-05, 5.9350541e-06, 9.9994111e-01,
            1.0736142e-06],
           [3.4572211e-05, 3.4797737e-05, 3.2656546e-05, 3.2816235e-05,
            9.9986517e-01]], dtype=float32)]
    **************************************************
    [3 4]
    input 4 is dataset/test/anqi.jpg, and the predict result is 安崎
    input 5 is dataset/test/wangchengxuan.jpg, and the predict result is 王承渲


    share_vars_from is set, scope is ignored.
    [32m[2020-04-26 13:08:42,793] [    INFO] - PaddleHub predict finished.[0m

