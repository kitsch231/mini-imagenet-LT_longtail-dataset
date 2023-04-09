# mini-imagenet-LT_longtail-dataset
完整数据集下载：https://huggingface.co/datasets/KITSCH/miniimagenet-LT/tree/main
长尾数据集的分类任务是一个较为常见的话题，但是数据集整理较为麻烦，并且有些数据集例如Imagenet-LT相对来说还是太多，算力不够的情况下做实验成本较高。因此我根据mini-Imagenet重新整理出了mini-Imagenet-LT长尾数据集。并且使用了RSG模型和stable diffusion扩充数据集两种方法进行性能上的对比。
RSG方法,allacc:72.62%  headacc:75.91%  middleacc:62.45%  tailacc:50.83%
SD方法,allacc:75.88%  headacc:79.36%  middleacc:64.31%  tailacc:56.25%

数据集整理过程如下：
1.下载原始mini-imagenet数据集，其由从imagenet中抽取的100个类别的数据构成，每个类别600张图片，总计60000张图片。我们从每个类别的图像中抽取10%的测试集10%的验证集，剩下80%作为训练集。测试集和验证集会生成val.csv和test.csv两个表格文件，记录了路径和标签。

2.为了制作长尾数据集我们需要对训练集进行再抽样。我们对每个类别的训练数据集从中随机抽取10到480不等的数据构成了分布不均匀的长尾数据集，生成train.csv文件，每个类别的数据量记录在cls_label.json。

3.使用stable diffusion扩充我们的长尾数据集，讲每个类别的图片数量从10-480补齐到480张，生成的图片在genimages文件夹加，标签路径文件为gentrain.csv。具体生成方法我们使用图生图的方式，以某图片及其标签作为prompt对现在的图片轮流生成直到补齐480张为止。（由于seed的随机性或图片的问题，生成的图片有部分为损坏的纯黑图片，在下游任务中记得做筛选去除）。语义标签保存在classname.txt中。

Complete dataset download: https://huggingface.co/datasets/KITSCH/miniimagenet-LT/tree/main
The classification task of long-tail data sets is a relatively common topic, but the data set sorting is more troublesome, and some data sets such as Imagenet-LT are relatively too much, and the cost of experimentation is high when the computing power is not enough. So I rearranged the mini-Imagenet-LT long-tail dataset based on mini-Imagenet. And use the RSG model and stable diffusion to expand the data set two methods for performance comparison.
RSG method, allacc: 72.62 headacc: 75.91 middleacc: 62.45 tailacc: 50.83
SD method, allacc: 75.88 headacc: 79.36 middleacc: 64.31 tailacc: 56.25

The process of organizing the data set is as follows:
1. Download the original mini-imagenet dataset, which consists of 100 categories of data extracted from imagenet, with 600 pictures for each category, and a total of 60,000 pictures. We sample 10% of the test set, 10% of the validation set, and the remaining 80% as the training set from images in each category. The test set and validation set will generate two table files, val.csv and test.csv, which record the path and label.

2. In order to make a long tail dataset we need to resample the training set. We randomly sampled 10 to 480 data from the training data set of each category to form an unevenly distributed long-tail data set, and generated a train.csv file. The data volume of each category is recorded in cls_label.json.

3. Use stable diffusion to expand our long-tail data set. The number of pictures in each category is filled from 10-480 to 480. The generated pictures are added in the genimages folder, and the label path file is gentrain.csv. For the specific generation method, we use the image generation method, using a certain image and its label as a prompt to generate the current images in turn until 480 images are completed. (Due to the randomness of the seed or the problem of the picture, some of the generated pictures are damaged pure black pictures, remember to filter and remove them in downstream tasks). Semantic tags are stored in classname.txt.
