# -AI-
# Author： Xueyi Shi
# 记录下个人以前的工程处理上一些技巧。AI项目，降维，逻辑结合方面的心得。
# 虽然已水表识别为例，其实该方法适合所有的仪器表的处理。

自动识别自来水表的类型（数字表、小齿轮等），分类出水表类型；识别数字、齿轮；并整合高低位输出最终的水表读数。

工程分逻辑处理部分和 CNN图像识别部分。
不同生成商制作的水表样式不同，但都可以切割出其数字部分和小齿轮部分。这些最小切割出来的部分具有高度的相似性，可以作为模型的输入。


整体思路：找出大表盘的整体部分出来。
        在大表盘内部 识别出 segmentation 块；
        根据segmentation块的整体空间分布,match其组合类型。
        
实施策略：只通过对一个数字表盘 （上面有5位数字 和 4个小齿轮）的摆拍，收集图片即可。（识别大表盘，可以通过网络收集几张量级的即可）。

采用数据增强技术. (以下涉及到图片输入的都可以通过该方式生成更多的增强图片用于训练）

任务划分：
1，切割出大表盘，只需要在网路上收集些水表，标注大表盘。
2，摆拍各种姿势的 数字+齿轮，标记处齿轮 和 小齿轮位置。根据数字大区域 和 小齿轮的位置。=》逻辑计算出正定后的图片。（数字区域位于中间正上方）

3.1，大数字部分（5个数字区域），训练单个0-9数字的小单元 => 切割出每个数字小单元。训练单个数字的识别。

单个数字的识别：调节数字的一个从0-9的过程，每个位置的图片对应数字的值，比如3-4之间分成10份，对应的值3.0-3.9
小齿轮的识别：和数字的方式一样。

实际中数字表盘的数字最后一位是0.1进度的，一般用红色表示。
这个简单，用HSV的cv2的API处理，如果红色mask > 一定阈值，表示其为红色。就可以区分出来了。



        
        
        
