# Multi-labels-Classification

- [this method reference1](https://blog.csdn.net/sushiqian/article/details/78763514)
- [this method reference2](https://blog.csdn.net/u013010889/article/details/54614067)
- [method2_1](https://blog.csdn.net/hubin232/article/details/50960201)
- [method2_2](https://blog.csdn.net/baobei0112/article/details/47606559)
- [method3](https://blog.csdn.net/Mr_Curry/article/details/54629129?locationNum=14&fps=1)
- [other method reference](https://blog.csdn.net/qq295456059/article/details/78428275)

## method1

### step 1.
change following three files:
```C++
   $CAFFE_ROOT/src/caffe/proto/caffe.proto

   $CAFFE_ROOT/include/caffe/layers/image_data_layer.hpp

   $CAFFE_ROOT/src/caffe/layers/image_data_layer.cpp
```
<br/>

### step 2.
CAFFE_ROOT_PATH/build complie

```C++
　 $CAFFE_ROOT/build$ make
```

if you using "make runtest" display error, you can ignore error message <br/>



### step 3.

**change train.prototxt / train.txt / val.txt / deploy.prototxt**
<br/>

**- train.prototxt**
for example:  4 class: ImageDate , Slice , loss_weight need be changed

```C++
layer {    
  name: "Input"    
  type: "ImageData"    
  top: "data"    
  top: "label"    
  include {    
    phase: TRAIN    
  }    
  transform_param {    
    scale: 0.00390625  
  }    
  image_data_param {    
    source: "/home/mark/data/train.txt"    
### root_folder: "/home/mark/data/"  ### according your train.txt images path   
    new_height: 60    
    new_width: 160    
    is_color: true    
    batch_size: 50    
    shuffle: true  
    label_dim: 4  
  }    
} 

----------------
layer {  
  name: "slice"  
  type: "Slice"  
  bottom: "label"  
  top: "label_1"  
  top: "label_2"  
  top: "label_3"  
  top: "label_4"  
  slice_param {  
    axis: 1  
    slice_point:1  
    slice_point:2  
    slice_point:3  
  }  
} 

---------------
.
.
.
layer {  
  name: "accuracy_1"  
  type: "Accuracy"  
  bottom: "ip3_4"  
  bottom: "label_1"  
  top: "accuracy_1"  
  include {  
    phase: TEST  
  }  
}  
layer {  
  name: "loss1"  
  type: "SoftmaxWithLoss"  
  bottom: "ip3_4"  
  bottom: "label_1"  
  top: "loss1"
  loss_weight: 0.25
} 

layer {  
  name: "accuracy2"  
  type: "Accuracy"  
  bottom: "ip3_4"  
  bottom: "label_2"  
  top: "accuracy2"  
  include {  
    phase: TEST  
  }  
}  
layer {  
  name: "loss2"  
  type: "SoftmaxWithLoss"  
  bottom: "ip3_4"  
  bottom: "label_2"  
  top: "loss2"
  loss_weight: 0.25
} 
.
.
.

```
<br/>

**- train.txt / val.txt**

```C++
YOUR_IMAGES_PATH/xxx_1.jpg 0 1 3 1
YOUR_IMAGES_PATH/xxx_2.jpg 1 1 0 1
YOUR_IMAGES_PATH/xxx_3.jpg 1 0 3 2
.
.
.
```
<br/>

**- deploy.prototxt**

```C++
.
.
.
layer {  
  name: "ip3_1"  
  type: "InnerProduct"  
  bottom: "ip2"  
  top: "ip3_1"  
  param {  
    lr_mult: 1  
  }  
  param {  
    lr_mult: 2  
  }  
  inner_product_param {  
    num_output: 10  
    weight_filler {  
      type: "xavier"  
    }  
    bias_filler {  
      type: "constant"  
    }  
  }  
}  
  
layer {  
  name: "ip3_2"  
  type: "InnerProduct"  
  bottom: "ip2"  
  top: "ip3_2"  
  param {  
    lr_mult: 1  
  }  
  param {  
    lr_mult: 2  
  }  
  inner_product_param {  
    num_output: 10  
    weight_filler {  
      type: "xavier"  
    }  
    bias_filler {  
      type: "constant"  
    }  
  }  
}  
  
layer {  
  name: "ip3_3"  
  type: "InnerProduct"  
  bottom: "ip2"  
  top: "ip3_3"  
  param {  
    lr_mult: 1  
  }  
  param {  
    lr_mult: 2  
  }  
  inner_product_param {  
    num_output: 10  
    weight_filler {  
      type: "xavier"  
    }  
    bias_filler {  
      type: "constant"  
    }  
  }  
}  
  
layer {  
  name: "ip3_4"  
  type: "InnerProduct"  
  bottom: "ip2"  
  top: "ip3_4"  
  param {  
    lr_mult: 1  
  }  
  param {  
    lr_mult: 2  
  }  
  inner_product_param {  
    num_output: 10  
    weight_filler {  
      type: "xavier"  
    }  
    bias_filler {  
      type: "constant"  
    }  
  }  
}  
  
layer {  
  name: "prob1"  
  type: "Softmax"  
  bottom: "ip3_1"  
  top: "prob1"  
}  
layer {  
  name: "prob2"  
  type: "Softmax"  
  bottom: "ip3_2"  
  top: "prob2"  
}  
layer {  
  name: "prob3"  
  type: "Softmax"  
  bottom: "ip3_3"  
  top: "prob3"  
}  
layer {  
  name: "prob4"  
  type: "Softmax"  
  bottom: "ip3_4"  
  top: "prob4"  
}  

```








