
# 设置分割线宽度

```objc
// 设置系统分割屏幕左右间隔15pt
if([self.tableView respondsToSelector:@selector(setCellLayoutMarginsFollowReadableWidth:)]) {
    if (@available(iOS 9.0, *)) {
        self.tableView.cellLayoutMarginsFollowReadableWidth = NO;
    }
}
if ([self.tableView respondsToSelector:@selector(setSeparatorInset:)]) {
    [self.tableView setSeparatorInset:UIEdgeInsetsMake(0, 15, 0, 15)];
}
if ([self.tableView respondsToSelector:@selector(setLayoutMargins:)]) {
    if (@available(iOS 8.0, *)) {
        [self.tableView setLayoutMargins:UIEdgeInsetsMake(0, 15, 0, 15)];
    }
}
```

# 拖拽表格取消键盘

```objc
// 拖拽表格取消键盘
self.tableView.keyboardDismissMode = UIScrollViewKeyboardDismissModeOnDrag;
```