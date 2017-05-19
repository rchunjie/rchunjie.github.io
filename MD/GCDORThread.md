### 浅学GCD
### 处理多次请求的时候 分组GCD

```
   dispatch_group_t group = dispatch_group_create();
    dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
    
    dispatch_group_async(group, queue, ^{
        NSLog(@"需要处理的1");
    });
    dispatch_group_async(group, queue, ^{
        NSLog(@"需要处理的2");
    });
    dispatch_group_async(group, queue, ^{
        NSLog(@"需要处理的3");
    });
    dispatch_group_async(group, queue, ^{
        NSLog(@"需要处理的4");
    });
    
    dispatch_group_notify(group, queue, ^{
        NSLog(@"搞定以后处理的位置");
    });
    
    
```

### 开辟子线程
```
    dispatch_async(dispatch_get_global_queue(0, 0), ^{
        //处理啊耗时慢的
        dispatch_async(dispatch_get_main_queue(), ^{
            //回到主线程//一般用于刷新
        });
    });
```

### 待补充

```
    dispatch_sync(dispatch_get_global_queue(0, 0), ^{
        NSLog(@"---1");
        NSLog(@"---3");
        NSLog(@"---2");
    });

```

 ### 八秒之后执行
 ```
    NSTimeInterval deInterval = 8;
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(deInterval * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        NSLog(@"我是8秒之后");
    });
```


### 线程依赖

```


    //1.任务一：下载图片
    NSBlockOperation *operation1 = [NSBlockOperation blockOperationWithBlock:^{
     [self PHFetchResult:smartAlbums withMArr:PHAssetMarr];
    }];
    //2.任务二：打水印
    NSBlockOperation *operation2 = [NSBlockOperation blockOperationWithBlock:^{
      [self PHFetchResult:topLevelUserCollections withMArr:PHAssetMarr];
    }];
    
    //3.任务三：上传图片
    NSBlockOperation *operation3 = [NSBlockOperation blockOperationWithBlock:^{
        /*
        dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(10 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
                photoBlock(PHAssetMarr,nil);
        });
        */
 
    }];
    
    //4.设置依赖
    [operation2 addDependency:operation1];      //任务二依赖任务一
    [operation3 addDependency:operation2];      //任务三依赖任务二
    

    //5.创建队列并加入任务
    NSOperationQueue *queue = [[NSOperationQueue alloc] init];
    [queue addOperations:@[operation3, operation2, operation1] waitUntilFinished:NO];

```


###  GCD


```
    dispatch_queue_t concurrentQueue = dispatch_queue_create("com.starming.gcddemo.concurrentqueue",DISPATCH_QUEUE_CONCURRENT);
    dispatch_group_t group = dispatch_group_create();
    dispatch_group_async(group, concurrentQueue, ^{
       [self PHFetchResult:smartAlbums withMArr:PHAssetMarr];
    });
    dispatch_group_async(group, concurrentQueue, ^{
      [self PHFetchResult:topLevelUserCollections withMArr:PHAssetMarr];
   });
   dispatch_group_notify(group, dispatch_get_main_queue(), ^{
        photoBlock(PHAssetMarr,nil);
    });
```

```
   dispatch_queue_t serialQueue = dispatch_queue_create("com.starming.gcddemo.serialqueue", DISPATCH_QUEUE_SERIAL);
   dispatch_block_t firstBlock = dispatch_block_create(0, ^{
       [self PHFetchResult:smartAlbums withMArr:PHAssetMarr];
       [self PHFetchResult:topLevelUserCollections withMArr:PHAssetMarr];
   });
    dispatch_async(serialQueue, firstBlock);

    dispatch_block_t secondBlock = dispatch_block_create(0, ^{
    photoBlock(PHAssetMarr,nil);
    });
    //first block执行完才在serial queue中执行second block
    dispatch_block_notify(firstBlock, serialQueue, secondBlock);

```

```
//属性
@property(nonatomic) dispatch_source_t timer;
 dispatch_queue_t queue = dispatch_get_global_queue(0, 0);
    
    self.timer = dispatch_source_create(DISPATCH_SOURCE_TYPE_TIMER, 0, 0, queue);
    
    //开始时间
    dispatch_time_t start = dispatch_time(DISPATCH_TIME_NOW, 3.0 * NSEC_PER_SEC);
    
    //间隔时间
    uint64_t interval = 1.0 * NSEC_PER_SEC;
    
    
    dispatch_source_set_timer(self.timer, start, interval, 0);
    
    //设置回调
    dispatch_source_set_event_handler(self.timer, ^{
        i ++;
        NSLog(@"----self.timer---%d",i);
        if (i == 10) {
         
        }
    });
    
    
    //启动timer
    dispatch_resume(self.timer);
    //暂停timer
    //dispatch_suspend(self.timer);
```