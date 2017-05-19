## 数组排序 NSSortDescriptor

### 数组中装RMStudensModel 模型 RMStudensModel 中有studentID 属性

```

 NSMutableArray * studensArr = [self creatMutableArr];
    
    
for (RMStudensModel * model in studensArr) {
   NSLog(@"%ld",model.studentID);
}
// 输出 132132
//ascending YES 升序 key 对象的属性
NSSortDescriptor * descr = [NSSortDescriptor sortDescriptorWithKey:@"studentID" ascending:YES];
 NSArray * tempArr = [studensArr sortedArrayUsingDescriptors:@[descr]];
for (RMStudensModel * model in tempArr) {
    NSLog(@"%ld",model.studentID);
}
    
 //输出112233


```

## 同样数组装同样的对象 sortUsingComparator

```
NSMutableArray * studens = [self creatMutableArr];
    //从大到小排列 <  从小到大排列  >
    [studens sortUsingComparator:^NSComparisonResult(id  _Nonnull obj1, id  _Nonnull obj2) {
        RMStudensModel * inter1 = obj1;
        RMStudensModel * inter2 = obj2;
        return inter1.studentID < inter2.studentID;
    }];
    
    [studens enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
        if ([obj isKindOfClass:[RMStudensModel class]]) {
                    RMStudensModel * model = obj;
            NSLog(@"%lu",model.studentID);
        }

    }];
    
```
    
## 几种循环
    
```
       NSMutableArray * strdens = [self creatMutableArr];
    
    for (int i =0 ; i< strdens.count -1; i++) {
        
    }
    
    
    for (RMStudensModel * model in strdens) {
        
    }
    
    [strdens enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
       
        if ([obj isKindOfClass:[RMStudensModel class]]) {
            * stop = YES;  //跳出循环
        }
    }];
    
```
## 数据源
    
```
    -(NSMutableArray *)creatMutableArr{
    
    NSMutableArray * studensArr = [NSMutableArray array];
    RMStudensModel * model1 = [[RMStudensModel alloc] init];
    model1.name = @"春节";
    model1.sex = YES;
    model1.studentID = 1;
    [studensArr addObject:model1];
    
    
    RMStudensModel * model2 = [[RMStudensModel alloc] init];
    model2.name = @"春节";
    model2.sex = YES;
    model2.studentID = 3;
    [studensArr addObject:model2];
    
    
    RMStudensModel * model3 = [[RMStudensModel alloc] init];
    model3.name = @"春节";
    model3.sex = YES;
    model3.studentID = 2;
    [studensArr addObject:model3];
    
    RMStudensModel * model4 = [[RMStudensModel alloc] init];
    model4.name = @"春节";
    model4.sex = YES;
    model4.studentID = 1;
    [studensArr addObject:model4];
    
    
    RMStudensModel * model5 = [[RMStudensModel alloc] init];
    model5.name = @"春节";
    model5.sex = YES;
    model5.studentID = 3;
    [studensArr addObject:model5];
    
    
    RMStudensModel * model6 = [[RMStudensModel alloc] init];
    model6.name = @"春节";
    model6.sex = YES;
    model6.studentID = 2;
    [studensArr addObject:model6];
    
    return studensArr;
    
}
```