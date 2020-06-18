
# 先 pop 再 push

```objc
-(void)pushToCurrentViewController:(UIViewController*)vc {
    
    NSMutableArray *vcArr = [NSMutableArray arrayWithArray:self.navigationController.viewControllers];
    
    for (NSInteger i = vcArr.count -1; i >= 0; i --) {
        UIViewController *indexVC = vcArr[i];
        
        if ([NSStringFromClass(indexVC.class) isEqualToString:NSStringFromClass(self.class)]) {
            [vcArr removeObjectAtIndex:i];
        }
    }
    
    [vcArr addObject:vc];
    [self.navigationController setViewControllers:vcArr animated:YES];
}
```