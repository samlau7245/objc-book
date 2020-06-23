

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






















