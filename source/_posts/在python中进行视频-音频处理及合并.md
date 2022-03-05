---
title: 在python中进行视频-音频处理及合并
categories: python
HTML格式实现标签样本: <center><font size=5 face="黑体">文本区</font></center>
date: 2020-03-29 10:56:44
top:
tags:
---



# 一、ffmpeg组件

ffmpeg是一套可以用来记录、转换数字音频、视频，并能将其转化为流的开源计算机程序。采用LGPL或GPL许可证。它提供了录制、转换以及流化音视频的完整解决方案。

**本次介绍ffmpeg的目的是：合并视频，合并音频，合并视频和音频。**

它可以直接在命令行下直接调用；也可以通过python程序调用。

## 1.1 下载及配置环境变量

下载地址：https://ffmpeg.zeranoe.com/builds/

下载完直接解压即可，将文件放到合适位置。

然后在path中配置环境变量。

配置完需要重启电脑才能生效。

如在CMD下输入以下命令，若有反应，则说明环境变量配置成功。

```
C:\Users\Aloha>ffmpeg -version
```

## 1.2 在python中使用

### 1.2.1 视频音频合并

```python
import subprocess  //这个不用pip安装，自带的

def video_add_audio(video_file, audio_file):
    """
     视频添加音频
    :param file_name: 传入视频文件的路径
    :param mp3_file: 传入音频文件的路径
    :return:
    """
    outfile_name = file_name + '-txt.mp4'
    subprocess.call('ffmpeg -i ' + video_file
                    + ' -i ' + audio_file + ' -strict -2 -f mp4 '
                    + outfile_name, shell=True)
```

### 1.2.2 视频合并

#### **方法一：FFmpeg concat 协议**

对于 MPEG 格式的视频，可以直接连接：

> ```
> ffmpeg -i "concat:input1.mpg|input2.mpg|input3.mpg" -c copy output.mpg
> ```

对于非 MPEG 格式容器，但是是 MPEG 编码器（H.264、DivX、XviD、MPEG4、MPEG2、AAC、MP2、MP3 等），可以包装进 TS 格式的容器再合并。在新浪视频，有很多视频使用 H.264 编码器，可以采用这个方法

> ```
> ffmpeg -i input1.flv -c copy -bsf:v h264_mp4toannexb -f mpegts input1.ts
> ffmpeg -i input2.flv -c copy -bsf:v h264_mp4toannexb -f mpegts input2.ts
> ffmpeg -i input3.flv -c copy -bsf:v h264_mp4toannexb -f mpegts input3.ts
> ffmpeg -i "concat:input1.ts|input2.ts|input3.ts" -c copy -bsf:a aac_adtstoasc -movflags +faststart output.mp4
> ```

保存 QuickTime/MP4 格式容器的时候，建议加上 `-movflags +faststart`。这样分享文件给别人的时候可以边下边看。

#### **方法二：FFmpeg concat 分离器**

这种方法成功率很高，也是最好的，但是需要 FFmpeg 1.1 以上版本。先创建一个文本文件`filelist.txt`：

> ```
> file 'input1.mkv'
> file 'input2.mkv'
> file 'input3.mkv'
> ```

然后：

> ```
> ffmpeg -f concat -i filelist.txt -c copy output.mkv
> ```

注意：使用 FFmpeg concat 分离器时，如果文件名有奇怪的字符，要在 `filelist.txt` 中转义。

## 1.3 用法总结

### 1.3.1 ffmpeg命令

命令格式：

```
ffmpeg -i [输入文件名] [参数选项] -f [格式] [输出文件]
ffmpeg [[options][`-i' input_file]]... {[options] output_file}...
```

-  参数选项：
    (1) -an: 去掉音频
    (2) -acodec: 音频选项， 一般后面加copy表示拷贝
    (3) -vcodec:视频选项，一般后面加copy表示拷贝
- 格式：
    (1) h264: 表示输出的是h264的视频裸流
    (2) mp4: 表示输出的是mp4的视频
    (3) mpegts: 表示ts视频流
- 注意：如果没有输入文件，那么视音频捕捉（只在Linux下有效，因为Linux下把音视频设备当作文件句柄来处理）就会起作用。作为通用的规则，选项一般用于下一个特定的文件。如果你给 –b 64选项，改选会设置下一个视频速率。对于原始输入文件，格式选项可能是需要的。缺省情况下，ffmpeg试图尽可能的无损转换，采用与输入同样的音频视频参数来输出。

**视频格式转换示例：**

```
//H264视频转ts视频流
ffmpeg -i test.h264 -vcodec copy -f mpegts test.ts

//ts视频转mp4
ffmpeg -i test.ts -acodec copy -vcodec copy -f mp4 test.mp4

//mp4视频转flv
ffmpeg -i test.mp4 -acodec copy -vcodec copy -f flv test.flv 
```

 **将视频转为音频：**

```
def video2mp3(file_name):
    """
    将视频转为音频
    :param file_name: 传入视频文件的路径
    :return:
    """
    outfile_name = file_name.split('.')[0] + '.mp3'
    subprocess.call('ffmpeg -i ' + file_name
                    + ' -f mp3 ' + outfile_name, shell=True)
```

- 分离视频：`ffmpeg -i 1.mp4  1.avi`
- 分离音频：`ffmpeg -i 1.mp4 1.mp3`



# 二、流媒体ts与m3u8

现在大部分视频网站为了反扒，会将视频切割成很多小段，形成后缀为.ts的视频文件。用户在网站上播放视频时，会一步步从服务器加载.ts视频进行播放。

**Ts文件视频编码主要格式h264/mpeg4，音频位aac/mp3**

与此同时，还会存在一个.m3u8文件，它是记录了一个索引纯文本文件，视频播放时，会根据m3u8的索引找到对应的音视频文件的网络地址进行在线播放。

**原视频数据分割为很多个TS流，每个TS流的地址记录在m3u8文件列表中。**

以下是m3u8文件的通常格式：

```python
#EXTM3U
#EXT-X-VERSION:3
#EXT-X-TARGETDURATION:13
#EXT-X-MEDIA-SEQUENCE:0
#EXT-X-KEY:METHOD=AES-128,URI=".20180125/key.key"  <--有这一行说明ts被加密了。
#EXTINF:12.5,
http://www.example.com/20180125/GBDYO3576000.ts   #<--这就是ts流视频的真实url
#EXTINF:12.5,
http://www.example.com/20180125/GBDYO3576001.ts
#EXTINF:12.5,
http://www.example.com/20180125/GBDYO3576002.ts
……………………
#EXT-X-ENDLIST
```

## 2.1 无加密

在.m3u8文件中，若无#EXT-X-KEY……一行字样，说明没有加密。

### 2.1.1 m3u8下载合并

**有了m3u8文件，可以直接使用ffmpeg进行下载，合并。**

注意这里的m3u8文件地址可以是网络url，也可以是本地ts文件路径。

```
ffmpeg -i file.m3u8 -c copy new.mp4
```

若报错： Protocol 'http' not on whitelist 'file,crypto,data'!

则执行下面这个：

```
ffmpeg -allowed_extensions ALL -protocol_whitelist "file,http,crypto,tcp" -i index.m3u8 -c copy out.mp4
```

### 2.1.2 ts合并

当我们已经直接把`.ts`文件下载到本地了，可以按照以下方法合并。

CMD下执行一下命令：

```cmd
copy/b D:\temp\*.ts D:\temp\output.ts
```

`D:\temp\*.ts` 意思是`D:\temp`目录下所有的`.ts`文件。

## 2.2 有加密

在.m3u8文件中，若存在#EXT-X-KEY……一行字样，说明.ts有加密。

```
#EXT-X-KEY:METHOD=AES-128,URI="key的地址"   
```

只要这一行有说明key的地址（不论本地，还是网络url），就可以直接执行：

```
ffmpeg -i file.m3u8 -c copy new.mp4
```


