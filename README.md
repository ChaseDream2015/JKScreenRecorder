# JKScreenRecorder
JKScreenRecorder(屏幕录制),use RPScreenRecorder of ReplayKit.framework  supports iOS9  and later, use AVAssetWriter of AVFoundation.framework supports iOS8

应用内屏幕录制功能,iOS9之后使用原生ReplayKit进行录制,iOS8及以后使用截图方式保存视频.

## 使用方法


开始录制
```
- (IBAction)startTouched:(id)sender {
    _sreenRecorder = [[JKScreenRecorder alloc]init];
    //不区分系统版本 直接使用截图合成视频
    //[_sreenRecorder startRecordingWithCapture];
    //根据不同版本的系统SDK 使用不同的API 两种开始方法调用一种即可
    [_sreenRecorder startRecordingWithHandler:^(NSError *error) {
        
    }];
    [_sreenRecorder screenRecording:^(NSTimeInterval duration) {
        self.timeLabel.text  = [@(duration) stringValue];
    }];
}

```
结束录制

```
- (IBAction)stopTouched:(id)sender {
    [_sreenRecorder stopRecordingWithHandler:^(UIViewController *previewViewController, NSString *videoPath, NSError *error) {
        
        //videoPath非空为使用低版本截图方式保存视频 previewViewController为MPMoviePlayerViewController的实例
        if (videoPath) {
            MPMoviePlayerViewController *p = (MPMoviePlayerViewController*)previewViewController;
            [p.moviePlayer prepareToPlay];
            [p.moviePlayer play];
            [self presentMoviePlayerViewControllerAnimated:p];
//            存入相册 注意要在info.plist中增加 Privacy - Photo Library Usage Description
//            ALAssetsLibrary *library = [[ALAssetsLibrary alloc] init];
//            [library writeVideoAtPathToSavedPhotosAlbum:[NSURL fileURLWithPath:videoPath]
//                                        completionBlock:^(NSURL *assetURL, NSError *error) {
//                                            if (error) {
//                                                NSLog(@"Save video fail:%@",error);
//                                            } else {
//                                                NSLog(@"Save video succeed.");
//                                            }
//                                        }];
        }else{
             //videoPath空为使用ReplayKit.framework方式录制视频 previewViewController为RPPreviewViewController的实例
            [self presentViewController:previewViewController animated:YES completion:^{
                
            }];
        }
            
    }];
}
```

## 效果
![](https://raw.githubusercontent.com/shaojiankui/JKScreenRecorder/master/demo.gif)


