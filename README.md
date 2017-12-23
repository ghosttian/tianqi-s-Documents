# tianqi-s-Documents
This respository collect the useful info during learning.

#特效协议说明
##Action
###BlurAction:
参数：blur_size，取值范围【0～20】，0：不做模糊；>20:根据经验，超过20模糊程度过大，不建议设置。支持动画：是。
###BeautyAction
参数：brightness，取值范围【1.0～2.0】,1:不加亮；2.0:根据经验，超过2.0纹理几乎呈白色，建议不要超过2.0。支持动画：否。
参数：smooth_degree，取值范围【0～1.0】，0:不做美颜；1.0:最大程度美颜。<0|>1为未知效果。支持动画：是。
###BlendAction
参数：blend_type，暂时支持23中混合类型。
参数：overlay_type：引用的资源纹理类型：支持image，stage和action，默认为action。
参数：overlay_id：引用纹理的id：如果overlay_type为image，overlay_id为对应的资源id；如果overlay_type为stage，overlay_id为对应的stage id；如果overlay_type为action，overlay_id为对应的action id。
###LayoutAction
参数：
