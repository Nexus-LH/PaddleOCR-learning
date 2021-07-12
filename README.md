# PaddleOCR-learning
参加天池“英特尔创新大师杯”深度学习挑战赛时，使用PaddleOCR实现通用场景OCR文本识别任务中出现的问题以及解决方案。
# 安装Paddle框架
我使用的是Anconda3，首先创建一个虚拟环境安装paddle以及后续所需要的库。
~~~
conda create -n paddle_env python=3.8 #创建虚拟环境
activate paddle_env #进入虚拟环境
conda install paddlepaddle --channel https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/Paddle/ #安装paddle
~~~
安装完成后可以使用 python进入python解释器，输入import paddle ，再输入 paddle.utils.run_check()出现PaddlePaddle is installed successfully!已成功安装。
# 克隆项目
[项目地址](https://gitee.com/coggle/tianchi-intel-PaddleOCR)
# 下载预测模型
由于OCR包括多个步骤，此时只对其中检测的部署进行fientune，所以其他部署的权重也需要下载。这里我是windows系统直接复制链接到浏览器下载。

( https://paddleocr.bj.bcebos.com/dygraph_v2.0/ch/ch_ppocr_server_v2.0_det_infer.tar
 https://paddleocr.bj.bcebos.com/dygraph_v2.0/ch/ch_ppocr_mobile_v2.0_cls_infer.tar
 https://paddleocr.bj.bcebos.com/dygraph_v2.0/ch/ch_ppocr_server_v2.0_rec_infer.tar)
# 安装requirements.txt中所需的库
~~~
cd C:\Users\Administrator\Desktop\天池\ocr竞赛\tianchi-intel-PaddleOCR #进入刚才克隆的项目文件夹内
pip install -r requirements.txt
~~~
在安装过程中出现Failed building wheel for python_Levenshtein，报这个错的主要原因是因为缺少whl文件。
解决方法在[此处](https://www.lfd.uci.edu/~gohlke/pythonlibs/#python-levenshtein)下载对应的whl文件安装上就行。

如图所示:![图片](https://github.com/Nexus-LH/PaddleOCR-learning/blob/main/1.png)

之后执行命令
~~~
pip install E:\Anconda\Anconda3\envs\paddle_env
~~~
# 预测
解压后用一张图片测试是否能预测成功，这里官网提供的是python3命令，需改为python.
~~~
python tools/infer/predict_system.py --image_dir="./1.jpg" --det_model_dir="./inference/ch_ppocr_server_v2.0_det_infer/"  --rec_model_dir="./inference/ch_ppocr_server_v2.0_rec_infer/" --cls_model_dir='./inference/ch_ppocr_mobile_v2.0_cls_infer/' --use_angle_cls=True --use_space_char=True
~~~
这时出现错误，
~~~
FileNotFoundError: Could not find module 'E:\Anconda\Anconda3\envs\paddle_env\Library\bin\geos_c.dll' (or one of its dependencies). Try using the full path with constructor syntax.
~~~
解决方案
~~~
conda install shapely
~~~
再运行，出现错误
~~~
not find model file path './inference/ch_ppocr_mobile_v2.0_cls_infer/'/inference.pdmodel
~~~
解决方案：将cls_model_dir的路径该为双引号即可。
~~~
--cls_model_dir='./inference/ch_ppocr_mobile_v2.0_cls_infer/' --> --cls_model_dir="./inference/ch_ppocr_mobile_v2.0_cls_infer/"
~~~
由于我使用的是cpu版本，需要在命令行后添加--use_gpu=False
最终命令如下：
~~~
python tools/infer/predict_system.py --image_dir="./1.jpg" --det_model_dir="./inference/ch_ppocr_server_v2.0_det_infer/"  --rec_model_dir="./inference/ch_ppocr_server_v2.0_rec_infer/" --cls_model_dir="./inference/ch_ppocr_mobile_v2.0_cls_infer/" --use_angle_cls=True --use_space_char=True --use_gpu=False
~~~
![](https://github.com/Nexus-LH/PaddleOCR-learning/blob/main/2.png)

说明预测成功
# 以上所有命令均在paddle_env这个虚拟环境中运行
