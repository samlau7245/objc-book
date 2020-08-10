
```objc
[Application] The app delegate must implement the window property if it wants to use a main storyboard file.
```

 启动新建项目时，黑屏的问题：

```objc
@interface AppDelegate : UIResponder <UIApplicationDelegate>
@property (strong, nonatomic) UIWindow *window;
@end
```

---

