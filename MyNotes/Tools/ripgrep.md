`rg` 是 **ripgrep** 的简称，是一个比 `grep` 更快的递归搜索工具，用 Rust 编写。

## **安装 ripgrep**

### **macOS**
```bash
brew install ripgrep
```

### **Ubuntu/Debian**
```bash
sudo apt install ripgrep
# 或使用 snap
sudo snap install ripgrep
```

### **Arch Linux**
```bash
sudo pacman -S ripgrep
```

### **通过 cargo (Rust)**
```bash
cargo install ripgrep
```

### **二进制下载**
```bash
# Linux
curl -LO https://github.com/BurntSushi/ripgrep/releases/latest/download/ripgrep_13.0.0_amd64.deb
sudo dpkg -i ripgrep_13.0.0_amd64.deb

# 或直接下载二进制
wget https://github.com/BurntSushi/ripgrep/releases/download/13.0.0/ripgrep-13.0.0-x86_64-unknown-linux-musl.tar.gz
tar xzf ripgrep-*.tar.gz
sudo cp ripgrep-*/rg /usr/local/bin/
```

## **基本语法**
```bash
rg [选项] 模式 [路径...]
```

## **与 grep 的主要区别**

| 特性 | **rg (ripgrep)** | **grep** |
|------|------------------|----------|
| **速度** | ⚡ 非常快 | 一般 |
| **默认递归** | ✅ 是 | ❌ 否 |
| **默认忽略.gitignore** | ✅ 是 | ❌ 否 |
| **彩色输出** | ✅ 优秀 | ⚠️ 一般 |
| **Unicode支持** | ✅ 完整 | ⚠️ 有限 |
| **二进制文件** | ❌ 默认跳过 | ⚠️ 可能乱码 |
| **PCRE2正则** | ✅ 支持 | ❌ 基本/扩展 |

## **常用示例**

### **1. 基本搜索**
```bash
# 在当前目录递归搜索
rg "pattern"

# 在指定目录搜索
rg "error" /var/log/

# 搜索多个模式
rg "error|warning|critical" app.log

# 正则表达式搜索
rg "^function\s+\w+"
```

### **2. 文件类型过滤**
```bash
# 只搜索 Python 文件
rg -t py "import"

# 只搜索 Markdown 文件
rg -t md "TODO"

# 排除 JavaScript 文件
rg -T js "console.log"

# 查看支持的文件类型
rg --type-list

# 常用类型：py, js, ts, java, c, cpp, rs, go, md, html, css, json, yml, toml
```

### **3. 智能搜索**
```bash
# 大小写智能搜索（默认）
rg "Error"  # 大写敏感
rg "error"  # 小写不敏感

# 强制大小写不敏感
rg -i "ERROR"

# 强制大小写敏感
rg -s "error"

# 全字匹配
rg -w "word"  # 匹配 "word"，不匹配 "password"

# 搜索字面字符串（禁用正则）
rg -F "foo.bar"  # 搜索字面值 "foo.bar"，不匹配正则
```

### **4. 上下文显示**
```bash
# 显示匹配行前后各2行
rg -C 2 "pattern"

# 显示匹配行前3行
rg -B 3 "pattern"

# 显示匹配行后1行
rg -A 1 "pattern"

# 显示行号
rg -n "pattern"

# 只显示文件名
rg -l "pattern"  # 包含匹配的文件
rg --files-without-match "pattern"  # 不包含匹配的文件
```

### **5. 忽略文件/目录**
```bash
# 默认已忽略 .gitignore 中的文件
rg "pattern"

# 包含 .gitignore 忽略的文件
rg -u "pattern"  # --unrestricted

# 包含隐藏文件
rg --hidden "pattern"

# 忽略特定目录
rg --glob '!node_modules' "pattern"
rg --glob '!*.min.js' "pattern"

# 只在特定文件中搜索
rg --glob '*.py' "pattern"
rg --glob 'src/**/*.rs' "pattern"
```

### **6. 高级搜索**
```bash
# 搜索替换（预览）
rg "old" --replace "new"

# 统计匹配数
rg -c "pattern"

# 搜索并排序结果
rg "pattern" | sort

# 搜索二进制文件（默认跳过）
rg -a "binary pattern"  # --text
```

## **实用场景**

### **1. 代码搜索**
```bash
# 查找函数定义
rg "^def\s+\w+"

# 查找类定义
rg "^class\s+\w+"

# 查找 TODO 注释
rg -t py "TODO:"  # Python
rg -t js "TODO:"  # JavaScript
rg -i "todo"      # 所有文件

# 查找未使用的导入（示例）
rg -t py "^import" | sort | uniq -c | grep "^\s*1"
```

### **2. 日志分析**
```bash
# 查找错误日志
rg -i "error|exception|fail" app.log

# 查找特定时间
rg "2024-01-15.*ERROR"

# 查找 IP 地址
rg "\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b"

# 统计错误出现次数
rg -c "ERROR" *.log
```

### **3. 配置搜索**
```bash
# 查找所有配置文件中的数据库连接
rg -t yaml -t json -t toml "database.*password"

# 查找所有环境变量设置
rg "^export\s+\w+="

# 查找所有 URL
rg "https?://[^\s]+"
```

### **4. 项目维护**
```bash
# 查找大文件中的文本
rg -a "needle" large_file.bin

# 查找重复代码（简化版）
rg -n "some repeated pattern" --sort path

# 查找硬编码的配置
rg -i "password|secret|token|key" --no-ignore
```

## **高级功能**

### **1. JSON 输出**
```bash
# JSON 格式输出，便于脚本处理
rg "pattern" --json

# 示例输出：
# {"type":"match","data":{"path":{"text":"file.txt"},"lines":{"text":"匹配的行内容\n"},"line_number":1,"absolute_offset":0,"submatches":[{"match":{"text":"pattern"},"start":10,"end":17}]}}
```

### **2. 搜索替换**
```bash
# 搜索并替换（输出到控制台）
rg "foo" --replace "bar"

# 实际替换文件内容（危险！）
# 使用 sed 更安全，或：
rg "foo" -l | xargs sed -i 's/foo/bar/g'
```

### **3. 管道处理**
```bash
# 搜索并将结果传递给其他命令
rg -l "pattern" | xargs wc -l

# 搜索并排序去重
rg -o "\w+@\w+\.\w+" | sort | uniq

# 统计每个文件的匹配数
rg -c "pattern" | sort -t: -k2 -nr
```

### **4. 多线程搜索**
```bash
# 指定线程数（默认自动）
rg --threads 4 "pattern"

# 使用所有核心
rg --threads 0 "pattern"
```

## **性能技巧**

### **1. 启用内存映射（更快）**
```bash
rg --mmap "pattern"  # 对大型文件更快
```

### **2. 限制搜索深度**
```bash
# 虽然 rg 没有直接的深度限制，但可用 glob 模式
rg --glob '*/'  # 当前目录一层
rg --glob '*/*/'  # 两层目录
```

### **3. 预过滤文件**
```bash
# 先按文件类型过滤，再搜索
rg -t py "pattern"  # 只搜索 Python 文件
```

## **配置文件**
```bash
# 配置文件位置
~/.ripgreprc
~/.config/ripgrep/rc

# 示例配置
--colors=line:fg:yellow
--colors=match:fg:red
--colors=path:fg:green
--smart-case
--hidden
```

## **与 grep 对比示例**

### **查找包含 "error" 的文件**
```bash
# grep 写法
grep -r "error" .

# rg 写法
rg "error"
```

### **查找 Python 文件中的函数**
```bash
# grep 写法
grep -r "^def " *.py

# rg 写法
rg -t py "^def "
```

### **统计匹配数**
```bash
# grep 写法
grep -r -c "TODO" .

# rg 写法
rg -c "TODO"
```

## **实用别名**
```bash
# 添加到 ~/.bashrc 或 ~/.zshrc

# 快速搜索并编辑
alias rge='rg -n --color=always | fzf --ansi --preview="bat --color=always {1} --highlight-line {2}" | cut -d: -f1,2 | xargs -o nvim +{2} {1}'

# 搜索内容并显示更多上下文
alias rgg='rg -C 3'

# 只显示匹配的文件名
alias rgl='rg -l'

# 搜索最近修改的文件
alias rgr='rg --files | xargs ls -lt | head -20'
```

## **ShellCheck 注意事项**
使用 `rg` 时，ShellCheck 的建议与 `grep` 类似：
```bash
# 使用引号防止特殊字符
rg "pattern with spaces"

# 处理特殊文件名
rg "pattern" -- "file with spaces.txt"

# 使用变量时要引号
search="my pattern"
rg "$search" file.txt
```

`rg` 是现代开发者的首选搜索工具，速度快、默认配置合理，特别是对代码搜索非常友好。