##锚点默认应该在什么位置？
锚点默认在中间位置。因此以锚点为中心的rotate，scale，translation都是围绕这个中心进行。

##关于rotate，scale，translation的实现
###实现方式1:采用Vertex Shader。
优点：实现方式方式简单，理论上效率更高。只需要顶点坐标乘以一个变换矩阵，即可同时进行这三种变换。

不足：有些复杂的效果难于实现。比如translation时，希望超出屏幕的纹理内容能够在空出的位置上进行填补时就没法实现。

###实现方式2:采用Fragment Shader。
优点：通过对纹理坐标进行处理，可以实现更为复杂的scale，translation操作。

不足：shader实现回稍有复杂，而且由于需要逐像素进行处理，理论上效率不高，尤其是在高分辨率，及性能较差的设备上会更加明显。

注意：在Fragment Shader中，纹理坐标处于屏幕坐标系，即x轴向右，y轴向下，坐标原点处于左上角。

###建议：对于这三种变换，以Vertex Shader为主，同时提供部分Fragment Shader以应付复杂的变换。

##对于现有MultiInput Action的修改建议
###独立出transform和blend两个shader
###将MultiInput中的多个输入纹理，分别通过transform shader和blend shader进行串联处理，以得到最终的FBO。

例如：

```
{
    "json_version": "0.1.0.0",
    "resource": {
    },
    "timeline": {
        "type": "momentary",
        "stages": [
                   ]
    },
    "actions": {
        "video_actions": [
                          {
                          "name": "SCBAction",
                          "id": "1",
                          "saturation": "-0.2",
                          "brightness": "-0.1"
                          },
                          {
                          "name": "GradientAction",
                          "id": "2",
                          "size": "10"
                          },
                          {
                          "name": "BlurAction",
                          "id": "3",
                          "size": "10"
                          },
                          {
                          "name": "MultipleInputsAction",
                          "id": "4",
                          "inputs": [
                                     {
                                     "id": "0",
                                     "spiritScale": 0.5,
                                     "spiritX": -0.5,
                                     "spiritY": -0.5
                                     },
                                     {
                                     "id": "1",
                                     "spiritScale": 0.5,
                                     "spiritX": 0.5,
                                     "spiritY": -0.5,
                                     },
                                     {
                                     "id": "2",
                                     "spiritScale": 0.5,
                                     "spiritX": -0.5,
                                     "spiritY": 0.5
                                     },
                                     {
                                     "id": "3",
                                     "spiritScale": 0.5,
                                     "spiritX": 0.5,
                                     "spiritY": 0.5
                                     }
                                     ]
                          }
                          ]
    }
}

```
将inputs中，第一项和第二项分别进行transform shader处理生成t1，t2，将t1，t2作为blend shader的输入并生成b1，再将第三项进行transform shader处理生成t3，将t3，b1作为blend shader输入生成b2，自后将第四项进行transform shader处理生成t4，将t4，b2作为blend shader输入生成b3。b3即为最终输出。这样，经过4次transform和3次blend后，即的到最终输出。

##需要统一的action
模糊：（BlurAction）
转换：（TransformAction）
视频叠加：（BlendAction）
图片叠加：（PictureBlendAction）
平移：（TranslationAction）
美颜：（BeautyAction）



