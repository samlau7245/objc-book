# 图层树

每个`UIView`都有一个`CALayer`实例的图层属性，用于创建和管理这个图层，以确保子视图在图层关系中添加或者被移除的时候，他们所关联的图层也同样在对应的层级树中进行相同的操作。

```objc
@interface UIView
@property(nonatomic,readonly,strong) CALayer  *layer;
@end
```

`UIView` 已经实现大部分的 `CALayer`功能，只有一些没有实现：

* 阴影、圆角、带颜色的边框。
* 3D变换。
* 非矩形范围。
* 透明遮罩。
* 多级非线性动画。

一个视图只有一个相关联的图层(自动创建)，同时它可以支持添加多个子图层。

```objc
@interface Ch1ViewController ()
@property (weak, nonatomic) IBOutlet UIView *layerView;// 视图
@end

@implementation Ch1ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    self.layerView.backgroundColor = UIColor.lightGrayColor;
    
    // 子图层
    CALayer *bluLayer = [CALayer layer];
    bluLayer.frame = CGRectMake(50.0f, 50.0f, 100.0f, 100.0f);
    bluLayer.backgroundColor = UIColor.blueColor.CGColor;
    [self.layerView.layer addSublayer:bluLayer];
}
@end
```

<img src="/assets/images/coreAnimation/01.png"/>

