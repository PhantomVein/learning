# FFmpeg添加视频内嵌字幕提示“Could not create a libass track when reading file”
## 错误提示
* Could not create a libass track when reading file 'xxx.ass'
* Error initializing filter 'ass' with args 'xxx.ass'
##解决办法
* 检查字幕文件的字符编码并改为UTF-8
