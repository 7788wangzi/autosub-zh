## FFMPEG常用操作命令

### 转格式
视频格式转换，使用`-i`选项指定输入文件，随后紧跟输出文件及后缀名。
```
ffmpeg -i input.mp4 output.avi
```

### 转码
转编码，将视频编码转换为H.264。 选项`-c:a`音频编码，缩写为`-acodec`， `-c:v`视频编码，缩写为`-vcodec`。
```
ffmpeg -i input.mp4 -c:a copy -c:v libx264 -r 24 -frames:v 24 output.mp4
```
转码时使用选项`-preset slow`可以确保转码过程耗时长，但文件小的效果。
比较复杂的转码命令
```
ffmpeg -y -f rawvideo -c:v rawvideo -pix_fmt rgb24 -r 60 -i input.mp4 \
        -preset slow -c:v libx264 -pix_fmt yuv420p -r 24 output.mp4
```

### 剪视频
输出选项`-ss`是新视频的起始时间， `-t`是设置新视频的时长。
```
ffmpeg -i input.mp4 -ss 00:00:00 -t 00:01:00 output.mp4
```
### 视频提取音频
作为输出选项的`-vn`标明no video; `-an`标明no audio。
```
ffmpeg -i input.mp4 -vn output.mp3
```
### 视频转GIF
作为输出选项的`-ss`标明GIF的起始时间， `-t`标明GIF的时长， `-i`输入源视频文件, `-s`设置GIF的分辨率大小， `-f gif`， `-r`设置每秒的帧数。
```
ffmpeg -ss 00:00:00 -t 00:00:10 -i input.mp4 -s 320x240 -f gif -r 1 output.gif
```

### 渲染字幕到视频中
需要注意的是只有MKV格式的容器支持渲染字幕，所以，如果是mp4的视频，需要先转格式到MKV，字幕渲染完成后，再转格式回到mp4。 
```
ffmpeg -i input.mp4 input.mkv
ffmpeg -i input.mkv -i input.srt output.mkv
ffmpeg -i output.mkv output.mp4
```