
# 两个时间对比

两个时间需要进行对比时，可以使用下面的方法：

```objc
- (NSDate *)earlierDate:(NSDate *)anotherDate;
- (NSDate *)laterDate:(NSDate *)anotherDate;
- (BOOL)isEqualToDate:(NSDate *)otherDate;

- (NSComparisonResult)compare:(NSDate *)other;
typedef NS_CLOSED_ENUM(NSInteger, NSComparisonResult) {
    NSOrderedAscending = -1L, // a < b
    NSOrderedSame, // a == b 
    NSOrderedDescending // a > b
};
```

# NSDate to NSString

```objc
-(NSDate*)getDate:(NSString*)dateString{
    NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
    [dateFormatter setDateFormat:@"yyyy-MM-dd"];
    return [dateFormatter dateFromString:dateString];
}
```

# NSString to NSDate

```objc
+ (NSString *)datePicker_formatterWithDate:(NSDate *)date {
    NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
    [dateFormatter setDateFormat:@"yyyy-MM-dd"];
    return [dateFormatter stringFromDate:date];
}
```
