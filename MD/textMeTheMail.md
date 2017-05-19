### 头文件及代理

```
#import <MessageUI/MessageUI.h>
#import <MessageUI/MFMailComposeViewController.h>
代理
<MFMessageComposeViewControllerDelegate,
MFMailComposeViewControllerDelegate>
```


### 打电话

```
-(void)makeCall
{
    UIWebView *phone = [[UIWebView alloc]initWithFrame:CGRectZero];
    [self.view addSubview:phone];
//
    NSString *urlStr = [NSString stringWithFormat:@"tel://%@",@"18796989587"];
    //self.myText.text是你要读入的电话号码，tel:不可省略，否则程序会出错
    NSURL *url = [NSURL URLWithString:urlStr];
    NSURLRequest *request = [NSURLRequest requestWithURL:url];
    [phone loadRequest:request];
    //底下这一种会直接拨打号码
    //[[UIApplication sharedApplication] openURL:url];
   /* ios10以后的写法
        [[UIApplication sharedApplication] openURL:[NSURL URLWithString:url]
        options:@{UIApplicationOpenURLOptionUniversalLinksOnly : @NO} completionHandler:^(BOOL success) {
        NSLog(@"%d",success);
    }];
    */

    
}
```
### 发消息

```
-(void)sendMessage
{
    if( [MFMessageComposeViewController canSendText] ){
        // MFMessageComposeViewController提供了操作界面,这里我们创建一个相应的控制器
        MFMessageComposeViewController * controller = [[MFMessageComposeViewController alloc]init];
        controller.recipients = [NSArray arrayWithObject:@"18796989587"];
        controller.body = @"测试发短信";
        controller.messageComposeDelegate = self;
        //显示发送信息界面的控制器
        [self presentViewController:controller animated:YES completion:nil];
    }else{
        NSLog(@"设备不具备短信功能");
    }
    [[self rac_signalForSelector:@selector(messageComposeViewController:didFinishWithResult:)
    fromProtocol:@protocol(MFMessageComposeViewControllerDelegate)] subscribeNext:^(RACTuple* x) {
        UIViewController*vc =x.first;
        MessageComposeResult result = x.second;
            [vc dismissViewControllerAnimated:YES completion:nil];
            if (result ==  MessageComposeResultSent) {
                [self.view makeToast:@"发送成功"];
            }else
            {
                [self.view makeToast:@"发送失败"];

            }
    }];
    //第二种方式
//    [[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"sms:18796989587"]];
}
```
### 发送邮件
```
-(void)sendEmail
{
    if ([MFMailComposeViewController canSendMail]) {
        MFMailComposeViewController* controller = [[MFMailComposeViewController alloc] init];
        controller.mailComposeDelegate = self;
        [controller setToRecipients:[NSArray arrayWithObjects:@"1769693301@qq.com", nil]];//self.myText.text为邮件地址
        //要发送的邮件主题
        [controller setSubject:@"邮件测试"];
        //要发送邮件的内容
        [controller setMessageBody:@"Hello " isHTML:NO];
        [self presentViewController:controller animated:YES completion:nil];
    }else{
        NSLog(@"设备不具备发送邮件功能");
    }
    
    [[self rac_signalForSelector:@selector(mailComposeController:didFinishWithResult:error:) fromProtocol:
    @protocol(MFMailComposeViewControllerDelegate)] subscribeNext:^(RACTuple* x) {
        UIViewController*vc =x.first;

        [vc dismissViewControllerAnimated:YES completion:nil];

    }];
}
```

