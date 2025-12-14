# Convert
`convert` 命令通常属于 **ImageMagick** 套件，是一个功能强大的图像处理工具。让我详细介绍：

## **安装 ImageMagick**

### **macOS**
```bash
brew install imagemagick
```

### **Ubuntu/Debian**
```bash
sudo apt-get install imagemagick
```

### **CentOS/RHEL**
```bash
sudo yum install imagemagick
```

## **基本语法**
```bash
convert [输入选项] 输入文件 [输出选项] 输出文件
```

## **常用功能示例**

### **1. 格式转换**
```bash
# JPEG 转 PNG
convert image.jpg image.png

# PNG 转 WebP
convert image.png image.webp

# PDF 转 JPEG（每页一张）
convert document.pdf page-%d.jpg

# 批量转换
convert *.jpg -set filename:f "%t" "png/%[filename:f].png"
```

### **2. 调整大小**
```bash
# 指定宽高
convert input.jpg -resize 800x600 output.jpg

# 保持宽高比（只指定宽度）
convert input.jpg -resize 800x output.jpg

# 只指定高度
convert input.jpg -resize x600 output.jpg

# 限制最大尺寸
convert input.jpg -resize 800x600\> output.jpg  # 仅当图片更大时缩小

# 强制拉伸到指定尺寸（可能变形）
convert input.jpg -resize 800x600! output.jpg

# 填充到指定尺寸（保持比例，填充背景）
convert input.jpg -resize 800x600 -background white -gravity center -extent 800x600 output.jpg
```

### **3. 图片质量**
```bash
# 调整 JPEG 质量（1-100）
convert input.jpg -quality 85 output.jpg

# 压缩 PNG
convert input.png -strip -quality 85 output.png

# 减小文件大小
convert large.jpg -define jpeg:extent=500kb output.jpg
```

### **4. 裁剪图片**
```bash
# 裁剪指定区域（宽x高+X偏移+Y偏移）
convert input.jpg -crop 300x200+100+50 output.jpg

# 裁剪为正方形
convert input.jpg -gravity center -crop 1:1 output.jpg

# 裁剪并调整大小
convert input.jpg -crop 1000x800+200+100 -resize 500x output.jpg
```

### **5. 旋转和翻转**
```bash
# 旋转 90 度
convert input.jpg -rotate 90 output.jpg

# 任意角度旋转（背景透明）
convert input.jpg -background transparent -rotate 45 output.png

# 水平翻转
convert input.jpg -flop output.jpg

# 垂直翻转
convert input.jpg -flip output.jpg
```

### **6. 添加效果**
```bash
# 添加边框
convert input.jpg -bordercolor black -border 10 output.jpg

# 添加圆角
convert input.jpg -alpha set -background none -vignette 10x10 output.png

# 黑白效果
convert input.jpg -grayscale Rec709Luma output.jpg

# 怀旧效果
convert input.jpg -sepia-tone 80% output.jpg

# 模糊效果
convert input.jpg -blur 0x8 output.jpg
convert input.jpg -gaussian-blur 5 output.jpg

# 锐化
convert input.jpg -sharpen 0x1 output.jpg
```

### **7. 添加文字水印**
```bash
convert input.jpg -pointsize 36 -fill white -stroke black -strokewidth 2 \
  -gravity southeast -annotate +10+10 "Copyright 2024" output.jpg

# 使用自定义字体
convert input.jpg -font /path/to/font.ttf -pointsize 24 -fill "#00000080" \
  -gravity center -annotate 0 "Watermark" output.jpg
```

### **8. 图片水印**
```bash
# 添加 Logo 水印
convert input.jpg watermark.png -gravity southeast -composite output.jpg

# 半透明水印
convert input.jpg \( watermark.png -resize 200x -alpha set -channel A -evaluate set 50% \) \
  -gravity center -composite output.jpg
```

### **9. 多图操作**
```bash
# 合并图片（垂直）
convert image1.jpg image2.jpg image3.jpg -append result.jpg

# 合并图片（水平）
convert image1.jpg image2.jpg image3.jpg +append result.jpg

# 创建网格
montage *.jpg -tile 3x3 -geometry +5+5 collage.jpg

# 创建 GIF
convert -delay 100 -loop 0 frame*.jpg animation.gif

# 拆分 GIF
convert animation.gif frame-%d.png
```

### **10. 获取图片信息**
```bash
# 显示图片信息
identify image.jpg
# 或
convert image.jpg -verbose info:

# 获取尺寸
identify -format "%wx%h" image.jpg

# 获取文件大小
identify -format "%b" image.jpg

# 获取更多信息
identify -format "尺寸: %wx%h\n格式: %m\n大小: %b\n颜色深度: %z\n" image.jpg
```

## **高级技巧**

### **批量处理脚本**
```bash
#!/bin/bash
# 批量调整图片大小
for img in *.jpg; do
    convert "$img" -resize 1200x800 "resized/${img%.jpg}_resized.jpg"
done

# 使用 mogrify（直接修改原文件）
mogrify -resize 800x600 *.jpg
```

### **创建缩略图**
```bash
# 创建正方形缩略图
convert input.jpg -thumbnail 150x150^ -gravity center -extent 150x150 thumbnail.jpg

# 批量创建
for img in *.jpg; do
    convert "$img" -thumbnail 200x200 "thumbs/${img}"
done
```

### **优化网页图片**
```bash
# 优化网页使用的图片
convert input.jpg -strip -interlace Plane -quality 85 -resize 800x output.jpg

# PNG 优化
convert input.png -strip -alpha remove -colors 256 output.png
```

### **创建特殊效果**
```bash
# 创建圆形头像
convert input.jpg -alpha set \( +clone -threshold 101% -fill white -draw "circle 100,100 100,0" \) \
  -compose DstIn -composite -trim output.png

# 添加阴影
convert input.jpg \( +clone -background black -shadow 80x3+5+5 \) \
  +swap -background white -layers merge +repage output.jpg
```

## **注意事项**

### **1. 安全警告（ImageMagick 安全策略）**
```bash
# 检查策略
convert -list policy

# 如果遇到 "not authorized" 错误，编辑配置文件
sudo vim /etc/ImageMagick-6/policy.xml
# 注释掉或修改限制策略
```

### **2. 性能优化**
```bash
# 使用合适的格式
convert large.tiff -compress JPEG -quality 90 output.jpg

# 处理前先缩小
convert large.jpg -resize 50% -quality 85 smaller.jpg

# 使用 -limit 限制资源
convert -limit memory 2GiB -limit disk 4GiB input.jpg output.jpg
```

### **3. 保持 EXIF 信息**
```bash
# 默认会删除 EXIF，使用 -strip 会明确删除
convert input.jpg output.jpg          # 可能保留部分 EXIF
convert input.jpg -strip output.jpg   # 明确删除所有元数据

# 保持所有元数据
convert input.jpg -auto-orient output.jpg
```

### **4. 色彩空间处理**
```bash
# 转换色彩空间
convert input.jpg -colorspace sRGB output.jpg

# 处理透明背景
convert input.png -background white -alpha remove output.jpg
```

## **替代命令**

ImageMagick 还提供了专门的命令：
- **`mogrify`**：批量处理并替换原文件
- **`composite`**：图片合成
- **`montage`**：创建图片网格
- **`identify`**：获取图片信息
- **`display`**：显示图片

## **ShellCheck 注意事项**
```bash
# 使用变量时要加引号
input="my image.jpg"
convert "$input" output.jpg

# 处理特殊文件名
convert "file with spaces.jpg" output.jpg
convert "file'with'quotes.jpg" output.jpg
```

`convert` 功能非常强大，是处理图片的瑞士军刀。掌握它可以在命令行中完成大部分图像处理任务，无需打开图形界面工具。