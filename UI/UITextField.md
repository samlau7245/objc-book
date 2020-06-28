

# 方法、属性

```objc
// 输入、placeholder 位置
@property(nonatomic) NSTextAlignment textAlignment;

typedef NS_ENUM(NSInteger, NSTextAlignment) {
    NSTextAlignmentLeft      = 0,    // Visually left aligned
    NSTextAlignmentCenter    = 1,    // Visually centered
    NSTextAlignmentRight     = 2,    // Visually right aligned
    NSTextAlignmentJustified = 3,    // Fully-justified. The last line in a paragraph is natural-aligned.
    NSTextAlignmentNatural   = 4     // Indicates the default alignment for script
}

```

# 常用解决方案
## 添加监听

```objc
[self.textFiledInput addTarget:self action:@selector(test:) forControlEvents:UIControlEventAllEditingEvents];

-(void)test:(UITextField*)sender{
    NSLog(@"test%@",sender.text);
    if (sender && sender.text && sender.text.length >= 4) {
        [sender resignFirstResponder];
    }
}
```

## 设置LeftView

```objc
UIView *paddingView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 5, 20)];
textField.leftView = paddingView;
textField.leftViewMode = UITextFieldViewModeAlways;
```

或者重写`UITextField`的方法：

```objc
@interface SATextField : UITextField
@end

@implementation SATextField
// placeholder position
- (CGRect)textRectForBounds:(CGRect)bounds {
     return CGRectInset(bounds, 10, 10);
}

// text position
- (CGRect)editingRectForBounds:(CGRect)bounds {
     return CGRectInset(bounds, 10, 10);
}
@end
```

## 设置输入单个字符之间的间距

* [Stack Overflow-How to set letter spacing of UITextField](https://stackoverflow.com/questions/31877002/how-to-set-letter-spacing-of-uitextfield)

```objc
// 设置监听

[self.textFiledInput addTarget:self action:@selector(editing:) forControlEvents:UIControlEventAllEditingEvents];

// 操作监听
-(void)editing:(UITextField*)sender{
    NSMutableAttributedString *attributedString = [[NSMutableAttributedString alloc] initWithString:sender.text];
    [attributedString addAttribute:NSFontAttributeName value:[UIFont systemFontOfSize:16] range:NSMakeRange(0, sender.text.length)];
    [attributedString addAttribute:NSKernAttributeName value:@5 range:NSMakeRange(0, sender.text.length)];
    if (sender.text.length > 3) {
        [attributedString addAttribute:NSKernAttributeName value:@5 range:NSMakeRange(0, 3)];
        [attributedString addAttribute:NSKernAttributeName value:@0 range:NSMakeRange(3, sender.text.length-3)];
    }else{
        [attributedString addAttribute:NSKernAttributeName value:@5 range:NSMakeRange(0, sender.text.length)];
    }
    sender.attributedText = attributedString;
}
// 限制输入字数
-(BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string{
    NSLog(@"%s",__FUNCTION__);
    NSString * str = [textField.text stringByReplacingCharactersInRange:range withString:string];
    if (str.length > 4) {
        return false;
    }
    return YES;
}
```

曾经做过的，短信验证码的逻辑:

```objc
// 操作监听
-(void)editing:(UITextField*)sender{
    NSMutableAttributedString *attributedString = [[NSMutableAttributedString alloc] initWithString:sender.text];
    [attributedString addAttribute:NSFontAttributeName value:TextFieldFont range:NSMakeRange(0, sender.text.length)];
    
    if (sender.text.length == 1) {
        CGFloat padding = (20- [self getSingleCharactorWidth:sender.text])/2;
        [self.inputTextField setValue:@(padding) forKey:@"paddingLeft"];
    }
    
    for (NSInteger i = 0; i < sender.text.length; i ++) {
        // 检查当前字符的下一个字符是否存在，如果存在取出来。
        NSInteger nextStringIndex = i+1;
        if (nextStringIndex >= sender.text.length) {
        }else{
            NSString *firstChar = [sender.text substringWithRange:NSMakeRange(i, 1)];
            NSString *secondChar = [sender.text substringWithRange:NSMakeRange(nextStringIndex, 1)];
            CGFloat kern = 5 + (20- [self getSingleCharactorWidth:firstChar])/2 + (20- [self getSingleCharactorWidth:secondChar])/2;
            [attributedString addAttribute:NSKernAttributeName value:@(kern) range:NSMakeRange(i, 1)];
        }
    }
    sender.attributedText = attributedString;
}
```

<img src="/assets/images/UI/03.gif"/>

计算的逻辑图：

<img src="/assets/images/UI/04.png"/>

## 限制输入的字符

```objc
// 限制输入字数
-(BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string{
    // 监听到 return 收起键盘
    if ([string isEqualToString:@"\n"]) {
        [textField resignFirstResponder];
        return NO;
    }
    
    if (!string || string.length <=0) {
        return YES;
    }
    
    NSString * str = [textField.text stringByReplacingCharactersInRange:range withString:string];
    if (str.length > 6) {
        return NO;
    }
    if (![@"0123456789Xx" containsString:string]) {
        return NO;
    }
    return YES;
}
```





















