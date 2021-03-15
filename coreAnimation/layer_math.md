
## 布局

布局属性：

| UIView| CALayer|描述|
| --- | --- | --- |
|frame|frame|表示图层外部坐标，也就是在父图层中占的位置|
|bounds|bounds| 表示图层内部坐标，{0,0} 表示从图层左上角|
|center|position|表示图层相对于父图层`锚点(anchorPoint)`所在的位置|

<img src="/assets/images/coreAnimation/09.png"/>

`frame`其实是个虚拟属性，它是根据`bounds`、`position`和`transform`计算而来，当其中任一值发生变化那么`frame`就会产生变化；相反`frame`发生变化也会影响它们的值。

当对图层做变换(旋转或缩放)的时候，`frame`和`bounds`中的宽高就不在一致了。

<img src="/assets/images/coreAnimation/10.png"/>

## 锚点(anchorPoint)

图层的锚点(anchorPoint)通过指定`position`来指定图层的`frame`。

