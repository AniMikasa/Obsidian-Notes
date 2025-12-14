`ffmpeg` 是一个强大的多媒体处理工具，用于视频、音频的录制、转换和流处理。以下是详细指南：

## **安装 ffmpeg**

### **macOS**
```bash
brew install ffmpeg
```

### **Ubuntu/Debian**
```bash
sudo apt install ffmpeg
```

### **CentOS/RHEL**
```bash
sudo yum install epel-release
sudo yum localinstall --nogpgcheck https://download1.rpmfusion.org/free/el/rpmfusion-free-release-7.noarch.rpm
sudo yum install ffmpeg ffmpeg-devel
```

### **Windows**
```bash
# 使用 Chocolatey
choco install ffmpeg

# 或从官网下载：https://ffmpeg.org/download.html
```

## **基本语法**
```bash
ffmpeg [全局选项] [输入文件选项] -i 输入文件 [输出文件选项] 输出文件
```

## **常用功能示例**

### **1. 格式转换**
```bash
# 视频格式转换
ffmpeg -i input.mp4 output.avi
ffmpeg -i input.mkv output.mp4

# 视频转 GIF
ffmpeg -i input.mp4 -vf "fps=10,scale=320:-1:flags=lanczos" output.gif

# 提取音频
ffmpeg -i video.mp4 -vn -acodec copy audio.aac
ffmpeg -i video.mp4 -vn -acodec mp3 audio.mp3

# 提取视频（无音频）
ffmpeg -i input.mp4 -an -vcodec copy output_video.mp4
```

### **2. 调整视频参数**
```bash
# 调整分辨率
ffmpeg -i input.mp4 -s 1280x720 output.mp4
ffmpeg -i input.mp4 -vf scale=1280:720 output.mp4

# 保持宽高比调整宽度
ffmpeg -i input.mp4 -vf scale=1280:-2 output.mp4

# 调整码率
ffmpeg -i input.mp4 -b:v 1M output.mp4
ffmpeg -i input.mp4 -b:v 1500k -maxrate 2000k -bufsize 1000k output.mp4

# 调整帧率
ffmpeg -i input.mp4 -r 30 output.mp4

# 调整速度
ffmpeg -i input.mp4 -filter:v "setpts=0.5*PTS" output.mp4  # 2倍速
ffmpeg -i input.mp4 -filter:v "setpts=2.0*PTS" output.mp4  # 0.5倍速
```

### **3. 裁剪和修剪**
```bash
# 按时间裁剪
ffmpeg -i input.mp4 -ss 00:01:00 -to 00:02:00 -c copy output.mp4
ffmpeg -i input.mp4 -ss 00:01:00 -t 10 -c copy output.mp4  # 从1分钟开始，截取10秒

# 裁剪画面区域
ffmpeg -i input.mp4 -filter:v "crop=w=640:h=480:x=100:y=100" output.mp4
ffmpeg -i input.mp4 -filter:v "crop=in_w/2:in_h/2:in_w/4:in_h/4" output.mp4

# 合并多个视频
echo "file 'part1.mp4'" > list.txt
echo "file 'part2.mp4'" >> list.txt
ffmpeg -f concat -i list.txt -c copy output.mp4
```

### **4. 音频处理**
```bash
# 调整音量
ffmpeg -i input.mp3 -af "volume=2.0" output.mp3
ffmpeg -i input.mp3 -af "volume=-10dB" output.mp3

# 标准化音量
ffmpeg -i input.mp3 -af loudnorm=I=-16:LRA=11:TP=-1.5 output.mp3

# 提取特定频段
ffmpeg -i input.mp3 -af "highpass=f=200, lowpass=f=3000" output.mp3

# 添加淡入淡出效果
ffmpeg -i input.mp3 -af "afade=t=in:st=0:d=3,afade=t=out:st=30:d=5" output.mp3
```

### **5. 添加/删除水印**
```bash
# 添加图片水印
ffmpeg -i input.mp4 -i watermark.png -filter_complex "overlay=10:10" output.mp4
ffmpeg -i input.mp4 -i watermark.png -filter_complex "overlay=(main_w-overlay_w)/2:(main_h-overlay_h)/2" output.mp4  # 居中

# 添加文字水印
ffmpeg -i input.mp4 -vf "drawtext=text='Copyright 2024':fontfile=/path/to/font.ttf:fontsize=24:fontcolor=white:x=10:y=10" output.mp4

# 添加时间戳水印
ffmpeg -i input.mp4 -vf "drawtext=fontfile=/path/to/font.ttf:text='%{localtime}':fontcolor=white:fontsize=24:box=1:boxcolor=black@0.5:boxborderw=5:x=10:y=10" output.mp4
```

### **6. 屏幕录制**
```bash
# Linux 屏幕录制
ffmpeg -f x11grab -video_size 1920x1080 -framerate 30 -i :0.0+100,200 output.mp4

# macOS 屏幕录制
ffmpeg -f avfoundation -i "1" -r 30 output.mp4
ffmpeg -f avfoundation -capture_cursor 1 -capture_mouse_clicks 1 -i "1" output.mp4

# Windows 屏幕录制
ffmpeg -f gdigrab -framerate 30 -i desktop output.mp4

# 同时录制屏幕和音频
ffmpeg -f avfoundation -i "1:0" -r 30 output.mp4  # macOS
ffmpeg -f dshow -i video="screen-capture-recorder":audio="virtual-audio-capturer" output.mp4  # Windows
```

### **7. 直播推流**
```bash
# 推流到 RTMP 服务器
ffmpeg -re -i input.mp4 -c copy -f flv "rtmp://server/live/stream_key"

# 从直播源录制
ffmpeg -i "rtmp://server/live/stream" -c copy -f segment -segment_time 3600 output_%03d.mp4

# YouTube 直播推流
ffmpeg -re -i input.mp4 -c:v libx264 -preset veryfast -maxrate 3000k -bufsize 6000k -pix_fmt yuv420p -g 50 -c:a aac -b:a 160k -ac 2 -ar 44100 -f flv "rtmp://a.rtmp.youtube.com/live2/stream_key"
```

### **8. 常用滤镜**
```bash
# 旋转视频
ffmpeg -i input.mp4 -vf "transpose=1" output.mp4
# 0=逆时针90°垂直翻转, 1=顺时针90°, 2=逆时针90°, 3=顺时针90°垂直翻转

# 镜像效果
ffmpeg -i input.mp4 -vf "hflip" output.mp4  # 水平镜像
ffmpeg -i input.mp4 -vf "vflip" output.mp4  # 垂直镜像

# 添加模糊效果
ffmpeg -i input.mp4 -vf "boxblur=10:5" output.mp4

# 调整亮度对比度
ffmpeg -i input.mp4 -vf "eq=brightness=0.1:contrast=1.5" output.mp4

# 去噪
ffmpeg -i input.mp4 -vf "hqdn3d=4:3:6:4.5" output.mp4

# 添加边框
ffmpeg -i input.mp4 -vf "pad=width=iw+20:height=ih+20:x=10:y=10:color=black" output.mp4
```

### **9. 批量处理**
```bash
# 批量转换 MP4 到 AVI
for f in *.mp4; do ffmpeg -i "$f" "${f%.mp4}.avi"; done

# 批量提取音频
for f in *.mp4; do ffmpeg -i "$f" -vn "${f%.mp4}.mp3"; done

# 批量调整分辨率
for f in *.mp4; do ffmpeg -i "$f" -s 1280x720 "resized/${f%.mp4}_720p.mp4"; done
```

## **实用技巧**

### **查看媒体信息**
```bash
# 详细媒体信息
ffmpeg -i input.mp4

# 查看流信息
ffprobe input.mp4

# 查看格式信息
ffprobe -v error -show_format input.mp4

# 查看流信息（JSON格式）
ffprobe -v quiet -print_format json -show_streams input.mp4

# 查看视频帧数
ffprobe -v error -select_streams v:0 -count_packets -show_entries stream=nb_read_packets -of csv=p=0 input.mp4

# 查看视频时长
ffprobe -v error -show_entries format=duration -of default=noprint_wrappers=1:nokey=1 input.mp4
```

### **编码器设置**
```bash
# 使用硬件加速（NVIDIA）
ffmpeg -hwaccel cuda -i input.mp4 -c:v h264_nvenc output.mp4

# 使用硬件加速（Intel）
ffmpeg -hwaccel qsv -i input.mp4 -c:v h264_qsv output.mp4

# 使用硬件加速（macOS）
ffmpeg -hwaccel videotoolbox -i input.mp4 -c:v h264_videotoolbox output.mp4

# 设置 CRF 质量（0-51，越小质量越好）
ffmpeg -i input.mp4 -c:v libx264 -crf 23 output.mp4

# 设置预设（编码速度/质量平衡）
ffmpeg -i input.mp4 -c:v libx264 -preset ultrafast output.mp4
ffmpeg -i input.mp4 -c:v libx264 -preset veryslow output.mp4
# ultrafast, superfast, veryfast, faster, fast, medium, slow, slower, veryslow
```

### **常用参数组合**
```bash
# 高质量转码
ffmpeg -i input.mp4 -c:v libx264 -preset slow -crf 18 -c:a aac -b:a 192k output.mp4

# 快速转码（低质量）
ffmpeg -i input.mp4 -c:v libx264 -preset ultrafast -crf 28 -c:a copy output.mp4

# 网页优化视频
ffmpeg -i input.mp4 -c:v libx264 -profile:v high -level 4.0 -pix_fmt yuv420p -movflags +faststart output.mp4

# 提取关键帧
ffmpeg -i input.mp4 -vf "select='eq(pict_type,PICT_TYPE_I)'" -vsync vfr keyframes_%03d.jpg
```

### **创建缩略图**
```bash
# 每秒提取一帧
ffmpeg -i input.mp4 -vf fps=1 thumbnails_%03d.jpg

# 提取所有关键帧
ffmpeg -i input.mp4 -vf "select='eq(pict_type,PICT_TYPE_I)'" -vsync vfr thumbnails_%03d.jpg

# 从指定时间提取单帧
ffmpeg -ss 00:01:23 -i input.mp4 -vframes 1 screenshot.jpg

# 创建缩略图网格
ffmpeg -i input.mp4 -vf "fps=1/60,scale=320:-1,tile=10x10" thumbnail_grid.jpg
```

## **常见问题解决**

### **1. 处理不兼容的编码**
```bash
# 强制重新编码
ffmpeg -i input.mov -c:v libx264 -c:a aac output.mp4

# 处理时间戳问题
ffmpeg -fflags +genpts -i input.mp4 -c copy output.mp4
```

### **2. 修复损坏的视频**
```bash
# 尝试修复
ffmpeg -i corrupt.mp4 -c copy repaired.mp4

# 忽略错误继续
ffmpeg -err_detect ignore_err -i input.mp4 -c copy output.mp4
```

### **3. 处理字幕**
```bash
# 硬编码字幕
ffmpeg -i input.mp4 -vf "subtitles=subtitle.srt" output.mp4

# 软字幕（嵌入）
ffmpeg -i input.mp4 -i subtitle.srt -c copy -c:s mov_text output.mp4

# 提取字幕
ffmpeg -i input.mkv -map 0:s:0 subtitle.srt
```

## **性能优化**

### **多线程处理**
```bash
# 使用多线程
ffmpeg -threads 4 -i input.mp4 output.mp4

# 自动多线程
ffmpeg -i input.mp4 -c:v libx264 -threads 0 output.mp4
```

### **内存优化**
```bash
# 限制内存使用
ffmpeg -i input.mp4 -c:v libx264 -preset ultrafast -threads 1 output.mp4

# 使用快速seek
ffmpeg -ss 00:10:00 -accurate_seek -i input.mp4 -to 00:11:00 -c copy output.mp4
```

## **使用注意事项**

1. **参数顺序很重要**：选项作用于后续文件
2. **`-c copy`**：不重新编码，直接复制流
3. **`-ss` 位置**：放在 `-i` 前更快但不精确，放在 `-i` 后更精确但慢
4. **测试命令**：先加 `-n` 参数测试不实际执行

`ffmpeg` 功能极其强大，几乎可以处理所有多媒体相关任务。掌握常用参数组合可以大大提高工作效率。