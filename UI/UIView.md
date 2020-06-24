
# 阴影

<img src="/assets/images/UI/02.gif"/>

从图中可以看出:**如果有圆角的UIView，阴影是无效的！** 因为一般设置圆角都设置了`masksToBounds`，补充一下两个属性：

```objc
// 是指视图上的子视图,如果超出父视图的部分就截取掉
@property(nonatomic) BOOL clipsToBounds;
// 却是指视图的图层上的子图层,如果超出父图层的部分就截取掉
@property BOOL masksToBounds;
```

如果想要圆角和阴影一起展示：可以不设置`masksToBounds`。
