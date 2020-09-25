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

# 寄宿图

## 寄宿图属性

```objc
@interface CALayer : NSObject
@property(nullable, strong) id contents;
@end
```

`contents` 类型是`id`，意味着它可以是任何类型的对象；但是在实践中如果类型不是`CGImage`类型，那么会得到一个空白的图层。

```objc
CALayer *bluLayer = [CALayer layer];
bluLayer.frame = CGRectMake(50.0f, 50.0f, 100.0f, 100.0f);
bluLayer.backgroundColor = UIColor.blueColor.CGColor;
[self.layerView.layer addSublayer:bluLayer];

UIImage *image = [UIImage imageNamed:@"bg_dialog_guaka"];

// 为什么要使用 __bridge
// Implicit conversion of C pointer type 'CGImageRef _Nullable' (aka 'struct CGImage *') to Objective-C pointer type 'id _Nullable' requires a bridged cast
bluLayer.contents = (__bridge id _Nullable)(image.CGImage);
```

<img src="/assets/images/coreAnimation/02.png"/>


### contentsGravity

```objc
@property(copy) CALayerContentsGravity contentsGravity;
```

`contentsGravity`的作用基本和`UIImageView.contentModel`类似，控制图片的的适应样式。

```objc
CA_EXTERN CALayerContentsGravity const kCAGravityCenter;
CA_EXTERN CALayerContentsGravity const kCAGravityTop;
CA_EXTERN CALayerContentsGravity const kCAGravityBottom;
CA_EXTERN CALayerContentsGravity const kCAGravityLeft;
CA_EXTERN CALayerContentsGravity const kCAGravityRight;
CA_EXTERN CALayerContentsGravity const kCAGravityTopLeft;
CA_EXTERN CALayerContentsGravity const kCAGravityTopRight;
CA_EXTERN CALayerContentsGravity const kCAGravityBottomLeft;
CA_EXTERN CALayerContentsGravity const kCAGravityBottomRight;
CA_EXTERN CALayerContentsGravity const kCAGravityResize;
CA_EXTERN CALayerContentsGravity const kCAGravityResizeAspect;
CA_EXTERN CALayerContentsGravity const kCAGravityResizeAspectFill;
```

把示例的图片适应模式设置为`kCAGravityResizeAspect`:

```objc
UIImage *image = [UIImage imageNamed:@"bg_dialog_guaka"];
bluLayer.contents = (__bridge id _Nullable)(image.CGImage);
bluLayer.contentsGravity = kCAGravityResizeAspect;
```

<img src="/assets/images/coreAnimation/03.png"/>

### contentsScale

```objc
@property CGFloat contentsScale;// default 1.0
```

`contentsScale`定义了寄宿图的像素尺寸和视图大小的比例。`contentsScale`属性其实是属于支持高分辨率屏幕机制的一部份。它用来判断在绘制图层时应为寄宿图创建的空间大小。`1.0`将为以每个点1像素绘制图片，`2.0`将会以每个点2像素绘制图片。这就是Retina屏。

```objc
UIImage *image = [UIImage imageNamed:@"bg_dialog_guaka"];
bluLayer.contents = (__bridge id _Nullable)(image.CGImage);
bluLayer.contentsGravity = kCAGravityCenter;
bluLayer.contentsScale = image.scale; // 设置这句和不设置的区别。
```

<img src="/assets/images/coreAnimation/04.png"/>

**如果代码处理寄宿图，一定要设置`contentsScale`，否则图片在Retain屏显示会不正确。**

### masksToBounds

```objc
@property BOOL masksToBounds;
```

`masksToBounds`属性决定是否显示超出边界的内容。

```objc
UIImage *image = [UIImage imageNamed:@"bg_dialog_guaka"];
bluLayer.contents = (__bridge id _Nullable)(image.CGImage);
bluLayer.contentsGravity = kCAGravityCenter;
bluLayer.contentsScale = image.scale;
bluLayer.masksToBounds = YES;
```

<img src="/assets/images/coreAnimation/05.png"/>

### contentsRect

```objc
@property CGRect contentsRect; // 值范围 [0,1] default (0,0,1,1)
```

`contentsRect`允许我们在图层边框里显示寄宿图的一个`子域？`。`contentsRect`使用了单位坐标系统，默认值为`(0,0,1,1)`这意味着整个寄宿图都是可见的，如果我们指定一个小一点的矩形。

```objc
UIImage *image = [UIImage imageNamed:@"bg_dialog_guaka"];
bluLayer.contents = (__bridge id _Nullable)(image.CGImage);
bluLayer.contentsGravity = kCAGravityCenter;
bluLayer.contentsScale = image.scale;
bluLayer.contentsRect = CGRectMake(0, 0, 0.5, 1);
```

<img src="/assets/images/coreAnimation/06.png"/>

iOS使用的坐标系统：

* `点(pt)`： 点像虚拟像素又被称为逻辑像素。在标准设备上一个点就是一个像素，在Retain屏上，一个点就是`2*2`或者`3*3`个像素。iOS用点作为屏幕坐标测量基准，就是为了在Retain设备和普通设备上能有一致的视觉效果。
* `像素(px)`： 
* `单位`： 单位坐标是指在0和1之间，是一个相对的值，对于与图片大小或者图层边界相关的显示。(点和像素是绝对值)


### 图片拼合(image sprites)

通过`contentsRect`可以实现图片的拼合。

```objc
@interface Ch1ViewController ()
@property (weak, nonatomic) IBOutlet UIView *oneView;
@property (weak, nonatomic) IBOutlet UIView *twoView;
@property (weak, nonatomic) IBOutlet UIView *threeView;
@property (weak, nonatomic) IBOutlet UIView *fourView;
@end

@implementation Ch1ViewController
- (void)viewDidLoad {
    [super viewDidLoad];
    UIImage *image = [UIImage imageNamed:@"bg_dialog_guaka"];
    [self addSpritesImage:image withContentRect:CGRectMake(0, 0, 0.5, 0.5) toLayer:self.oneView.layer];
    [self addSpritesImage:image withContentRect:CGRectMake(0.5, 0, 0.5, 0.5) toLayer:self.twoView.layer];
    [self addSpritesImage:image withContentRect:CGRectMake(0, 0.5, 0.5, 0.5) toLayer:self.threeView.layer];
    [self addSpritesImage:image withContentRect:CGRectMake(0.5, 0.5, 0.5, 0.5) toLayer:self.fourView.layer];
}
-(void)addSpritesImage:(UIImage*)image withContentRect:(CGRect)rect toLayer:(CALayer*)layer {
    layer.contents = (__bridge id _Nullable)(image.CGImage);
    // scale contents to fit
    layer.contentsGravity = kCAGravityResizeAspect;
    // set contentRect
    layer.contentsRect = rect;
}
@end
```

<img src="/assets/images/coreAnimation/07.png"/>

### contentsCenter

```objc
@property CGRect contentsCenter; // default (0,0,1,1)
```

`contentsCenter`定义了一个固定的边框和一个在图层上可拉伸的区域。改变`contentsCenter`的值并不会影响到寄宿图的展示，除非这个图层的大小改变。`contentsCenter`的默认值`(0,0,1,1)`，这意味这图层改变了，寄宿图将会均匀的展开。

<img src="/assets/images/coreAnimation/08.png"/>

效果类似与`UIImage`中的方法：

```objc
- (UIImage *)resizableImageWithCapInsets:(UIEdgeInsets)capInsets;
- (UIImage *)resizableImageWithCapInsets:(UIEdgeInsets)capInsets resizingMode:(UIImageResizingMode)resizingMode;
```

## Custom Drawing

给图层设置寄宿图不仅仅可以通过设置`contents`属性来实现，也可以通过`Core Graphics`直接绘制寄宿图；通过`- (void)drawRect:(CGRect)rect;`来实现自定义绘制寄宿图图。

### 关于 drawRect

`-drawRect:`方法是没有默认实现的，对于`UIView`来说寄宿图并不是必须的，如果`UIView`检测到`-drawRect:`方法被调用了，它就会为视图分配一个寄宿图，这个`寄宿图的尺寸大小等于视图大小*contentScale`。

如果`UIView`不需要寄宿图，那就不要创建这个方法，这会造成CPU资源和内存的浪费。

### CALayer

```objc
@protocol CALayerDelegate <NSObject>
@optional
- (void)displayLayer:(CALayer *)layer;
- (void)drawLayer:(CALayer *)layer inContext:(CGContextRef)ctx;
@end
```

# 图层几何学

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






















































































