# 数据集获取
在网上获取你想要的图片，可以是动漫也可以是真人，如果是脸型训练推荐10-15张，如果是动漫全身训练推荐25-30张。推荐选取背景干净的图片。
![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228205221.png)
# 数据集预处理
进入以下网站将刚刚获取的图片分辨率剪切成512 * 512或者768 * 768后再保存到新的文件夹中。

https://www.birme.net/?target_width=512&target_height=512
![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228205427.png)

## 打标插件

https://github.com/picobyte/stable-diffusion-webui-wd14-tagger
https://github.com/toshiaki1729/stable-diffusion-webui-dataset-tag-editor


请先将以上两个插件安装在extensions目录下，用于后续打标。
![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228203720.png)

### Tagger
先使用tagger插件，选择批量导入图片，输入图片路径和保存输出路径，然后点击标签生成。生成后会在保存输出路径下生成对应的txt文件。
![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228203904.png)

之后将图片文件复制到输出路径和txt文件放在一个文件夹下，二者一一对应。
![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228204400.png)

### Dataset Tag Editor
再使用Dataset Tag Editor插件，导入刚刚处理好的文件夹。
![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228204625.png)

接着对tag进行处理

1）删除重复的单词，Remove duplicate tags

2）删除人物特征类tag，比如人物的眼睛、眉毛、鼻子、头发长度等代表人物本身的属性。凡是绑定在人物身上的，就要把它们删除。（因为后续我们需要根据lora名称直接生成这些特征，所以需要模型根据lora名称直接学到这些特征，而不需要再提供其他提示词）

![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228204736.png)
tag处理完成之后点击Save all changes，保存处理完的tag文本文件。
![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228204920.png)

# Lora模型训练
考虑到很多家用电脑无法满足Lora训练的要求，我们推荐使用Google Colab进行微调，Colab登录连接后会为我们分配免费GPU。
我们点击以下链接进入写好的Colab脚本。
https://colab.research.google.com/github/hollowstrawberry/kohya-colab/blob/main/Lora_Trainer.ipynb
![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228210019.png)
首先为你的项目创建一个名字，在projec_name框中输入，我这里直接取名为 Asuka_Lora.
## 数据集上传
1.接着我们需要在Google Drive中创建 *Loras*文件夹。  
2.进入Lora文件夹后创建一个项目文件的同名(我的就是Asuka_Lora)的文件夹。  
3.然后在这个文件夹下创建一个*dataset*文件夹。  
4.最后将本地包含tag和图片的文件夹上传到dataset文件夹中。

![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228210718.png)

## 训练设置
接着你可以自行选择训练脚本的模型基底、超参数和优化器。如果是训练动漫人物就选择Anime模型，如果是训练人像就选择sd-v1.5模型。
![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228211059.png)
超参数可自由配置，但是新手建议使用默认配置即可。
![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228211339.png)
在选择完毕后一键点击左上角的播放按钮即可进行训练。
![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228211500.png)

## 训练检测
在最下方内置了自带的模型训练看板TensorBoard，点击即可打开TensorBoard，获取过程信息。

![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228211708.png)


## 权重下载
训练完毕后我们回到项目同名文件夹下，会发现多了一个output文件夹
![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228212003.png)点开后就是我们训练完毕的Lora模型权重文件，将它们全部下载到本地。再统一复制到Lora文件夹内，具体路径如下。
![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228212909.png)

# Lora模型实测
我们选择一个动漫风格的模型底座，再选择一个Lora，可以选择序号靠后的，因为靠后轮次的模型通常loss更低，效果更好。
![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228214210.png)

再输入通用风格的提示词如master pieces，high quality，然后生成即可。可以看到效果还是非常不错的。
![image.png](https://kashiwa-pic.oss-cn-beijing.aliyuncs.com/20240228214618.png)

我们可以通过反复更改Lora的权重和提示词来获取更好的效果，在这里就不多做赘述了。