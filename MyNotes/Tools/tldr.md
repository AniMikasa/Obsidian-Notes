`tldr` 是一个简化的命令帮助工具，提供**实用示例**而非完整手册页。

## **安装 tldr**

### **macOS**
```bash
brew install tldr
```

### **Ubuntu/Debian**
```bash
sudo apt install tldr
```

### **通过 npm**
```bash
npm install -g tldr
```

### **通过 pip**
```bash
pip install tldr
```

### **直接下载客户端**
```bash
# 手动安装（Linux/macOS）
curl -o /usr/local/bin/tldr https://raw.githubusercontent.com/raylee/tldr/master/tldr
chmod +x /usr/local/bin/tldr
```

## **基本用法**
```bash
tldr [命令名称]
```

## **常用功能**

### **1. 查看命令示例**
```bash
# 查看常见命令的用法示例
tldr tar
tldr find
tldr ffmpeg
tldr docker
```

### **2. 平台特定查看**
```bash
# 查看不同平台的用法
tldr --platform linux find      # Linux 版本
tldr --platform osx find        # macOS 版本
tldr --platform windows find    # Windows 版本
tldr --platform sunos find      # SunOS 版本
```

### **3. 搜索命令**
```bash
# 搜索相关命令
tldr --search "archive"         # 搜索与压缩归档相关的命令
tldr --search "network"         # 搜索网络相关命令
```

### **4. 列出所有可用命令**
```bash
tldr --list                     # 列出所有可用的命令页面
tldr --list | grep git          # 列出所有 git 相关命令
```

### **5. 更新本地缓存**
```bash
tldr --update                   # 更新命令示例数据库
tldr --clear-cache              # 清除本地缓存
```

## **与 man 的区别**

| 特性 | **tldr** | **man** |
|------|----------|---------|
| **内容** | 实用示例 | 完整手册 |
| **长度** | 1-2页 | 通常很长 |
| **重点** | 常用场景 | 所有选项 |
| **可读性** | 简单直观 | 技术性强 |
| **搜索** | 按命令名 | 复杂搜索 |
| **更新** | 社区维护 | 官方发布 |

## **实际示例对比**

### **查看 tar 命令的帮助**
```bash
# tldr 输出示例
$ tldr tar

tar

Archiving utility.
Often combined with a compression method, such as gzip or bzip2.

- Create an archive from files:
  tar cf target.tar file1 file2 file3

- Create a gzipped archive:
  tar czf target.tar.gz file1 file2 file3

- Extract an archive in a target directory:
  tar xf source.tar -C directory

- Extract a gzipped archive in the current directory:
  tar xzf source.tar.gz

- Extract a bzipped archive in the current directory:
  tar xjf source.tar.bz2

- Create a compressed archive, using archive suffix to determine the compression program:
  tar caf target.tar.xz file1 file2 file3

- List the contents of a tar file:
  tar tvf source.tar
```

```bash
# man tar 的输出（仅开头部分）
$ man tar | head -20

TAR(1)                                                                   TAR(1)

NAME
       tar - an archiving utility

SYNOPSIS
       tar [OPTION...] [FILE]...

DESCRIPTION
       GNU  'tar'  saves many files together into a single tape or disk archive, 
       and can restore individual files from the archive.
       
       The first argument to tar should be a function; either one of the letters
       Acdrtux, or one of the long function names.
       
       ...
```

## **配置 tldr**

### **配置文件位置**
```bash
# 配置文件路径
~/.tldrrc
~/.config/tldr/config.json
```

### **常用配置选项**
```json
{
  "colors": {
    "text": "white",
    "command": "green",
    "parameter": "yellow"
  },
  "platform": "linux",
  "language": "zh",
  "repository": "https://github.com/tldr-pages/tldr",
  "cache_dir": "~/.cache/tldr"
}
```

## **与其他工具集成**

### **与 Shell 集成**
```bash
# 在 ~/.bashrc 或 ~/.zshrc 中添加别名
alias help='tldr'

# 自动补全支持
# Bash
mkdir -p ~/.bash_completion.d
tldr --completion bash > ~/.bash_completion.d/tldr

# Zsh
mkdir -p ~/.zsh/completions
tldr --completion zsh > ~/.zsh/completions/_tldr
```

### **与 fzf 结合**
```bash
# 使用 fzf 搜索和查看 tldr 页面
tldr --list | fzf --preview "tldr {}" --preview-window=right:70%
```

### **网页版 tldr**
```bash
# 访问在线版本
# https://tldr.inbrowser.app/
# https://tldr.ostera.io/

# 或者使用命令行打开网页
tldr --render https://tldr.sh/assets/tldr-book.pdf
```

## **高级用法**

### **1. 语言支持**
```bash
# 查看支持的语言
tldr --list-languages

# 使用中文页面（如果可用）
tldr --language zh tar
tldr --language zh_CN tar

# 设置默认语言
export TLDR_LANGUAGE=zh
tldr tar
```

### **2. 自定义源**
```bash
# 使用自定义的 tldr 页面源
export TLDR_REPOSITORY="https://raw.githubusercontent.com/your-username/tldr-pages/master"
tldr --update

# 本地源
export TLDR_REPOSITORY="file:///path/to/local/tldr-pages"
```

### **3. 输出格式**
```bash
# 纯文本输出
tldr --raw tar

# Markdown 格式
tldr --markdown tar

# JSON 格式
tldr --json tar
```

### **4. 网络代理**
```bash
# 设置代理
export HTTP_PROXY="http://proxy.example.com:8080"
export HTTPS_PROXY="http://proxy.example.com:8080"
tldr --update
```

## **实用技巧**

### **创建常用命令速查表**
```bash
# 生成个人常用命令速查表
echo "# 我的命令行速查表" > cheatsheet.md
echo "## 压缩解压" >> cheatsheet.md
tldr --markdown tar >> cheatsheet.md
tldr --markdown zip >> cheatsheet.md
echo "## 网络" >> cheatsheet.md
tldr --markdown curl >> cheatsheet.md
tldr --markdown wget >> cheatsheet.md
```

### **作为脚本的一部分**
```bash
#!/bin/bash
# 在脚本中显示命令帮助
if [[ "$1" == "--help" ]]; then
    tldr "$0"
    exit 0
fi
```

### **批量更新**
```bash
# 更新所有已缓存的页面
tldr --update
```

### **离线使用**
```bash
# tldr 默认缓存页面，可以离线使用
# 先在有网络时更新
tldr --update

# 然后在离线环境下使用
tldr tar
```

## **替代工具**

### **cheat**
```bash
# 安装
pip install cheat

# 使用
cheat tar
```

### **navi**
```bash
# 交互式速查工具
brew install navi

# 使用
navi
```

### **bro**
```bash
# 另一个 tldr 客户端
brew install bro
bro tar
```

## **贡献 tldr 页面**
```bash
# 1. 克隆仓库
git clone https://github.com/tldr-pages/tldr.git

# 2. 创建新页面
cd tldr
cp pages/common/tar.md pages/common/newcommand.md

# 3. 编辑页面
vim pages/common/newcommand.md

# 4. 提交 PR
```

`tldr` 是学习新命令或快速回忆命令用法的绝佳工具，特别适合初学者和需要快速参考的开发人员。