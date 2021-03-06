V0.01 实现8齿轮的机械自来水表盘的读数AI识别。
由于正常的读取用水数据，精确到个位。而水表的每个小齿轮的读数在是向前还是向后，依赖于下一位的读数。
实战中，为了读数的精确，小数点后一位也自动识别；这样需要其后的一位齿轮也要识别，来辅助该位的精确识别。


标图工具：via2.0

主流程步骤：
1，识别出水表整个大表盘。（该模型的训练图像只标出大表盘）
   水表照片，由于距离的因素，可能导致占照片的整体区域比较小，导致其内部的小齿轮更小。识别出大表盘之后，裁剪下来。
   ![./images-folders/0001.jpg](https://github.com/sht06019/-AI-/blob/自来水表AI识别读数/images-folder/0001.jpg)
   切割下大表盘
   ![./images-folders/0001_mask.jpg](https://github.com/sht06019/-AI-/blob/自来水表AI识别读数/images-folder/0001_mask.jpg)
   
2, 识别出8小齿轮的顺序。（该模型的训练图片标注前6个小齿轮，标注顺序从高位到低位，直接表示了rank的顺序。然后选取前4个高位为categoryA, 后2低位为categoryB）
   这样出入裁剪后的大表盘，输出是 4个高位的categoryA, 和2个低位的categoryB。然后它们围成的顺序排序，得到上图所示的rank1-6的。
   
3，每个小齿轮的识别。（这个可以流程化快速实现训练集的构建。）
   ![./image-folders/07.jpg](https://github.com/sht06019/-AI-/blob/自来水表AI识别读数/images-folder/07.jpg)
   快速制作的训练集的步骤：（购置一个这样的水表，然后拆开，自己手动从00-99位置按照100等分小齿轮的一圈，拍摄不同角度、远近的图片。或者拍视频然后从中选取帧图片,然后用1,2步的模型裁剪出07对应的小齿轮图片）
   将识别出来的小齿轮依次识别出其指针相对于一圈100等分对应位置的值。按照当前位由下一位的读数共同读出读数。
  
  
  
代码库运行步骤。
通过Flask起 http的API接口

用tkinter制作了一个简易的UI来调用
效果如图：
   ![./image-folders/demo.jpg](https://github.com/sht06019/-AI-/blob/自来水表AI识别读数/images-folder/demo.jpg)
   
  
