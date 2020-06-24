
# UIImage指定图片可变区域（slicing)

## 视图化操作

<img src="/assets/images/UI/01.png"/>

## 代码操作

```objc
- (UIImage *)resizableImageWithCapInsets:(UIEdgeInsets)capInsets;
- (UIImage *)resizableImageWithCapInsets:(UIEdgeInsets)capInsets resizingMode:(UIImageResizingMode)resizingMode;

typedef NS_ENUM(NSInteger, UIImageResizingMode) {
    UIImageResizingModeTile = 0, // 平铺模式，通过重复显示UIEdgeInsets指定的矩形区域来填充图片
    UIImageResizingModeStretch = 1, // 拉伸模式，通过拉伸UIEdgeInsets指定的矩形区域来填充图片
};
```

示例：

```objc
UIEdgeInsets insets = UIEdgeInsetsMake(30,47,15,10);
UIImage *image =[[UIImage imageNamed:@"icons8-folder"] resizableImageWithCapInsets:insets resizingMode:UIImageResizingModeStretch];
UIImageView *imageView=[[UIImageView alloc]initWithFrame:CGRectZero];
imageView.image=image;
[self.view addSubview:imageView];
imageView.frame = CGRectMake(10, 100, 250, 100);
```
