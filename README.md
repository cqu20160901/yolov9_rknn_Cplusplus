# yolov9_rknn_Cplusplus

yolov9 瑞芯微 rknn 板端 C++部署，使用平台 rk3588。模型转换参考 [【yolov9 瑞芯微芯片rknn部署、地平线芯片Horizon部署、TensorRT部署】](https://blog.csdn.net/zhangqian_1/article/details/136321979)  。

模型文件比较大存放路径[【rknn模型链接】](https://github.com/cqu20160901/yolov9_rknn_Cplusplus/releases/tag/v1.0.0)

## 编译和运行

1）编译

```
cd examples/rknn_yolov9_demo_dfl_open

bash build-linux_RK3588.sh

```

2）运行

```
cd install/rknn_yolov9_demo_dfl_open

./rknn_yolov9_demo

```

注意：修改模型、测试图像、保存图像的路径，修改文件为src下的main.cc

```

int main(int argc, char **argv)
{
    char model_path[256] = "/home/zhangqian/rknn/rknpu2_1.4.0/examples/rknn_yolov9_demo_dfl_open/model/RK3588/yolov9_relu_80class_zq.rknn";
    char image_path[256] = "/home/zhangqian/rknn/rknpu2_1.4.0/examples/rknn_yolov9_demo_dfl_open/test.jpg";
    char save_image_path[256] = "/home/zhangqian/rknn/rknpu2_1.4.0/examples/rknn_yolov9_demo_dfl_open/test_result.jpg";

    detect(model_path, image_path, save_image_path);
    return 0;
}
```


# 测试效果


冒号“:”前的数子是coco的80类对应的类别，后面的浮点数是目标得分。（类别:得分）

![images](https://github.com/cqu20160901/yolov9_rknn_Cplusplus/blob/main/examples/rknn_yolov9_demo_dfl_open/test_result.jpg)


说明：训练代码参考官方开源的yolov9训练代码，考虑到有些板端对SiLU的支持有限，本示例训练前把激活函数SiLU替换成了ReLU，训练使用的模型配置文件是yolov9.yaml，输入分辨率640x640。用 from thop import profile 统计的模型计算量和参数 Flops: 120081612800.0（120G），Params: 55388336.0（55M）

把板端模型推理和后处理时耗也附上供参考，使用的芯片rk3588，模型输入640x640，检测类别80类。

![17090077736846](https://github.com/cqu20160901/yolov9_rknn_Cplusplus/assets/22290931/8f55cb09-b02b-40cf-876b-ed1f1a78d3a5)


