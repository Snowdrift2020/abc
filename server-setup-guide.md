# 服务器工作环境配置 & Linux 常用命令速查

> 适用环境:Ubuntu Server(本机为 `Ubuntu4EDA1`),登录用户 xxx
> 本文档涵盖:工作目录规划、Claude Code 用户级安装、常用 Linux 命令(带参数说明)

---

## 目录

- [一、工作目录规划与配置](#一工作目录规划与配置)
- [二、Claude Code 用户级安装](#二claude-code-用户级安装)
- [三、常用 Linux 命令详解](#三常用-linux-命令详解)
- [附录:本次配置一次性命令包](#附录本次配置一次性命令包)

---

## 一、工作目录规划与配置

### 1.1 目录结构设计

```
~/
├── snap/              ← 系统自动创建,不要动
├── work/              ← 主工作区(SSH 登录后自动进入)
│   ├── projects/      ← 正式项目,每个子目录一个 git 仓库
│   ├── playground/    ← 试验、学习、临时 demo
│   ├── scripts/       ← 跨项目复用的小脚本(已加入 PATH)
│   ├── notes/         ← 笔记、规划、设计文档(.md)
│   ├── tools/         ← 自己编译/下载的工具二进制
│   └── tmp/           ← 临时文件、下载、解压垃圾场
└── .claude/           ← Claude Code 用户配置(自动生成)
```

### 1.2 各目录用途约定

| 目录 | 放什么 | 不放什么 |
|---|---|---|
| `projects/` | 每个子目录都是独立项目,用 git 管理 | 单文件、零散脚本 |
| `playground/` | 试新技术、写 demo、学习练手 | 长期维护的代码 |
| `scripts/` | 跨项目复用的小工具,已加入 PATH 全局可调 | 项目专属脚本 |
| `notes/` | markdown 笔记、设计文档、待办 | 代码 |
| `tools/` | 手动安装的二进制、源码编译产物 | apt 装的东西 |
| `tmp/` | 能随时 `rm -rf` 的东西 | 任何重要文件 |

### 1.3 创建目录

```bash
mkdir -p ~/work/{projects,playground,scripts,notes,tools,tmp}
ls -la ~/work
```

### 1.4 登录自动进入工作目录 + PATH 配置

```bash
echo 'cd ~/work' >> ~/.bashrc
echo 'export PATH=~/work/scripts:$PATH' >> ~/.bashrc
source ~/.bashrc
```

验证:

```bash
pwd                                        # 应显示 /home/liuxiaolong/work
echo $PATH | tr ':' '\n' | grep scripts    # 应能看到 work/scripts
```

### 1.5 使用习惯建议

1. **新项目**:`cd ~/work/projects && mkdir <项目名> && cd <项目名> && git init`
2. **`tmp/` 定期清理**:不要在那里放重要东西
3. **EDA 仿真产物**(波形、log、综合中间文件)放项目内的 `build/` 或 `sim/`,加入 `.gitignore`
4. **可执行脚本**扔进 `scripts/`,记得 `chmod +x`,之后任何目录都能直接调用

---

## 二、Claude Code 用户级安装

> 推荐方式:**用户级 npm 前缀**,避免 sudo 权限污染、避免缓存属主混乱。

### 2.1 (可选) 先卸载全局版本

如果之前已经用 `sudo npm install -g` 装过:

```bash
sudo npm uninstall -g @anthropic-ai/claude-code
which claude          # 应无输出
```

### 2.2 配置 npm 用户级前缀

```bash
mkdir -p ~/.npm-global
npm config set prefix ~/.npm-global
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

执行后:

- 全局 npm 包将装到 `~/.npm-global/lib/node_modules/`
- 可执行命令链接放在 `~/.npm-global/bin/`(已加入 PATH)
- **以后所有 `npm install -g` 都不再需要 sudo**

### 2.3 安装 Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

### 2.4 验证与启动

```bash
which claude            # 应显示 ~/.npm-global/bin/claude
claude --version        # 显示版本号
claude                  # 启动,首次会引导登录
```

### 2.5 用户配置位置

Claude Code 运行时数据(与安装位置分离):

| 内容 | 路径 |
|---|---|
| 配置 / 登录态 | `~/.claude/` |
| 项目级会话历史 | `~/.claude/projects/` |
| 全局设置 | `~/.claude/settings.json` |

**备份这一个目录就保留所有用户态**,换机器/重装直接拷贝过去即可。

### 2.6 常见问题

**Q: `EACCES: permission denied`(`/usr/lib/node_modules`)**
A: 你还在用系统级 npm 前缀。执行 2.2 节配置用户级前缀。

**Q: `command not found: claude`**
A: 检查 `echo $PATH` 是否包含 `~/.npm-global/bin`,没有就 `source ~/.bashrc`。

**Q: 旧的全局 npm 缓存属主是 root**
A: `sudo chown -R $(whoami) ~/.npm` 改回。

---

## 三、常用 Linux 命令详解

> 格式:命令 — 功能 — 常用参数 — 实例

### 3.1 文件与目录

#### `ls` — 列出目录内容
| 参数 | 含义 |
|---|---|
| `-l` | 长格式(权限、属主、大小、时间) |
| `-a` | 显示隐藏文件(以 `.` 开头) |
| `-h` | 文件大小人类可读(K/M/G) |
| `-t` | 按修改时间排序 |
| `-r` | 反向排序 |
| `-S` | 按文件大小排序 |
| `-R` | 递归显示子目录 |
| `-d` | 只显示目录本身,不进入 |
| `-i` | 显示 inode 号 |

```bash
ls -lah                  # 最常用组合
ls -ltrh                 # 按时间倒序,最新的在最下
ls -ld /var/log          # 只看 /var/log 目录信息
```

#### `cd` — 切换目录
```bash
cd                       # 回到 home (~)
cd ~                     # 同上
cd -                     # 回到上一个目录
cd ..                    # 上一级
cd ../..                 # 上两级
cd /                     # 根目录
```

#### `pwd` — 显示当前路径
```bash
pwd                      # /home/liuxiaolong/work
pwd -P                   # 解析所有软链接,显示真实路径
```

#### `mkdir` — 创建目录
| 参数 | 含义 |
|---|---|
| `-p` | 递归创建,父目录不存在自动建,已存在不报错 |
| `-m MODE` | 指定权限(如 `-m 755`) |
| `-v` | 显示每个创建动作 |

```bash
mkdir -p a/b/c/d                    # 一口气建 4 层
mkdir -p ~/work/{src,bin,doc}       # brace 展开,一次建多个
mkdir -m 700 secret                 # 创建并设置权限
```

#### `rm` — 删除文件/目录 ⚠️
| 参数 | 含义 |
|---|---|
| `-r` 或 `-R` | 递归删除目录 |
| `-f` | 强制删除,不提示 |
| `-i` | 每次删除前确认 |
| `-v` | 显示删除过程 |

```bash
rm file.txt
rm -rf dir/              # 递归强删,危险!
rm -i *.log              # 逐个确认
```

> **危险命令警告**:`rm -rf /` 或 `rm -rf $VAR/`(当 `$VAR` 为空时)会删根。删之前先 `ls` 看一眼。

#### `cp` — 复制
| 参数 | 含义 |
|---|---|
| `-r` 或 `-R` | 递归复制目录 |
| `-i` | 覆盖前确认 |
| `-f` | 强制覆盖 |
| `-p` | 保留权限/时间戳/属主 |
| `-a` | 归档模式,等于 `-dR --preserve=all` |
| `-v` | 显示过程 |
| `-u` | 仅当源文件更新时才复制 |
| `-n` | 不覆盖已存在文件 |

```bash
cp file.txt backup.txt
cp -r src/ backup/                  # 复制整个目录
cp -av project/ /backup/project/    # 完整备份
```

#### `mv` — 移动 / 重命名
| 参数 | 含义 |
|---|---|
| `-i` | 覆盖前确认 |
| `-f` | 强制覆盖 |
| `-n` | 不覆盖已存在 |
| `-v` | 显示过程 |

```bash
mv old.txt new.txt                  # 重命名
mv file.txt /tmp/                   # 移动到 /tmp
mv *.log logs/                      # 批量移动
```

#### `ln` — 创建链接
| 参数 | 含义 |
|---|---|
| `-s` | 符号链接(软链接,推荐) |
| `-f` | 覆盖已存在的链接 |
| `-v` | 显示过程 |

```bash
ln -s /opt/tool/bin/foo ~/work/scripts/foo    # 软链接
ln file.txt hardlink.txt                       # 硬链接(不加 -s)
```

> 硬链接和软链接区别:硬链接共享 inode,删原文件链接仍可用;软链接是路径快捷方式,原文件删了链接就坏(dangling)。**日常用 `-s` 就行。**

#### `touch` — 创建空文件 / 更新时间戳
```bash
touch new.txt                       # 不存在则创建
touch -a file.txt                   # 只更新访问时间
touch -m file.txt                   # 只更新修改时间
touch -t 202601011200 file.txt      # 指定时间(YYYYMMDDhhmm)
```

#### `stat` — 查看文件详细信息
```bash
stat file.txt                       # 显示 inode、大小、权限、各种时间
stat -c '%n %s %y' *                # 自定义格式输出
```

---

### 3.2 查看文件内容

#### `cat` — 拼接 / 输出文件内容
| 参数 | 含义 |
|---|---|
| `-n` | 显示行号 |
| `-b` | 显示行号(空行不编号) |
| `-A` | 显示所有不可见字符($、^I 等) |
| `-s` | 多个空行压缩为一个 |

```bash
cat a.txt b.txt > merged.txt        # 拼接两文件
cat -n script.sh                    # 带行号
cat << EOF > config.txt             # heredoc 写入
line1
line2
EOF
```

#### `less` — 分页查看(推荐用这个看大文件)
```bash
less /var/log/syslog
```
**交互按键**:
- `空格` / `f` — 下一页
- `b` — 上一页
- `g` — 文件开头
- `G` — 文件末尾
- `/关键词` — 向下搜索,`n` 下一个,`N` 上一个
- `?关键词` — 向上搜索
- `q` — 退出

#### `head` / `tail` — 看头部/尾部
| 参数 | 含义 |
|---|---|
| `-n N` | 行数(默认 10) |
| `-c N` | 字节数 |
| `-f` | (tail 专属)持续追踪文件追加 |

```bash
head -n 20 file.txt
tail -n 50 log.txt
tail -f /var/log/syslog             # 实时监控日志(最常用)
tail -F file.log                    # 文件被轮转后仍能追踪
```

#### `wc` — 统计行/词/字节
| 参数 | 含义 |
|---|---|
| `-l` | 只数行数 |
| `-w` | 只数单词数 |
| `-c` | 只数字节数 |
| `-m` | 字符数(多字节按字符计) |

```bash
wc -l *.py                          # 每个 .py 文件行数
ls | wc -l                          # 当前目录文件数
```

---

### 3.3 搜索与查找

#### `find` — 按条件查找文件(强大但语法繁)
| 参数 | 含义 |
|---|---|
| `-name PAT` | 文件名匹配(区分大小写) |
| `-iname PAT` | 文件名匹配(不区分大小写) |
| `-type f/d/l` | 类型:文件/目录/链接 |
| `-size +10M` | 大于 10M(`-` 是小于,`+` 是大于) |
| `-mtime -7` | 7 天内修改过 |
| `-mtime +30` | 30 天前修改的 |
| `-user NAME` | 属主匹配 |
| `-perm 644` | 权限匹配 |
| `-empty` | 空文件/目录 |
| `-maxdepth N` | 最大递归深度 |
| `-exec CMD {} \;` | 对每个结果执行命令 |
| `-delete` | 删除匹配项 |

```bash
find . -name "*.log"                                 # 当前目录递归找 .log
find /var/log -type f -mtime -1                      # 24h 内的文件
find . -type f -size +100M                           # 大于 100M 的文件
find . -name "*.tmp" -delete                         # 找到并删除
find . -name "*.sh" -exec chmod +x {} \;             # 批量加执行权限
find . -type d -empty                                # 空目录
```

#### `grep` — 按内容搜索文本
| 参数 | 含义 |
|---|---|
| `-i` | 忽略大小写 |
| `-r` 或 `-R` | 递归搜索目录 |
| `-n` | 显示行号 |
| `-l` | 只显示匹配的文件名 |
| `-L` | 只显示不匹配的文件名 |
| `-v` | 反向匹配(不含的) |
| `-c` | 统计匹配行数 |
| `-w` | 整词匹配 |
| `-A N` | 显示匹配行后 N 行 |
| `-B N` | 显示匹配行前 N 行 |
| `-C N` | 前后各 N 行 |
| `-E` | 扩展正则(等同 egrep) |
| `-F` | 固定字符串(等同 fgrep,更快) |
| `--include='*.py'` | 仅搜某类文件 |
| `--exclude-dir=node_modules` | 排除某目录 |

```bash
grep "error" log.txt
grep -rn "TODO" src/                                 # 递归找 TODO,带行号
grep -i --include='*.c' "malloc" -r .                # 只搜 C 文件
grep -v "^#" config.conf                             # 去掉注释行
ps aux | grep nginx                                  # 配合管道
```

#### `which` / `whereis` / `type`
```bash
which python3            # 显示 PATH 中第一个匹配的可执行文件
whereis python3          # 显示二进制、源码、man 路径
type ls                  # 显示是别名/内置/外部命令
```

---

### 3.4 权限管理

Linux 文件权限 9 位:`rwx rwx rwx`(owner / group / other)。

#### `chmod` — 修改权限
**符号模式**:
```bash
chmod u+x file           # owner 加执行权限
chmod g-w file           # group 去掉写权限
chmod o=r file           # other 设为只读
chmod a+r file           # 所有人加读权限(a = ugo)
```

**数字模式**(常用):
| 数字 | 权限 | 含义 |
|---|---|---|
| 7 | rwx | 读写执行 |
| 6 | rw- | 读写 |
| 5 | r-x | 读执行 |
| 4 | r-- | 只读 |
| 0 | --- | 无 |

```bash
chmod 755 script.sh                   # rwxr-xr-x(可执行文件常用)
chmod 644 config.txt                  # rw-r--r--(普通文件常用)
chmod 700 ~/.ssh                      # rwx------(私密目录)
chmod -R 755 dir/                     # 递归
```

#### `chown` / `chgrp` — 改属主/属组
| 参数 | 含义 |
|---|---|
| `-R` | 递归 |
| `--reference=FILE` | 参照另一个文件的属主 |

```bash
sudo chown user:group file
sudo chown -R liuxiaolong:liuxiaolong ~/work
sudo chgrp developers project/
```

---

### 3.5 文本处理

#### `sed` — 流编辑器
| 参数 | 含义 |
|---|---|
| `-i` | 直接修改原文件(危险,建议先 `-i.bak` 备份) |
| `-n` | 静默,只输出 `p` 命令打印的内容 |
| `-e` | 多个表达式 |
| `-E` | 扩展正则 |

```bash
sed 's/old/new/' file.txt                      # 每行第一个 old 替换为 new
sed 's/old/new/g' file.txt                     # 全部替换
sed -i.bak 's/foo/bar/g' file.txt              # 直接改文件,保留 .bak 备份
sed -n '10,20p' file.txt                       # 打印第 10-20 行
sed '/^#/d' config.conf                        # 删除注释行
```

#### `awk` — 列处理神器
```bash
awk '{print $1}' file.txt                      # 第 1 列
awk '{print $NF}' file.txt                     # 最后一列
awk -F: '{print $1}' /etc/passwd               # 用 : 分隔,取第 1 列
awk '$3 > 100 {print $0}' data.txt             # 第 3 列 > 100 的行
awk 'NR==5' file.txt                           # 第 5 行
ps aux | awk '{print $2, $11}'                 # PID 和命令
```

#### `sort` — 排序
| 参数 | 含义 |
|---|---|
| `-n` | 按数值 |
| `-r` | 反向 |
| `-u` | 去重(等价 `sort | uniq`) |
| `-k N` | 按第 N 列 |
| `-t SEP` | 指定分隔符 |
| `-h` | 人类可读数字(1K, 2M)排序 |

```bash
sort file.txt
sort -nr scores.txt                            # 按数值倒序
sort -t: -k3 -n /etc/passwd                    # 按 UID 排
du -h | sort -h                                # 按大小排
```

#### `uniq` — 去重(必须先排序)
| 参数 | 含义 |
|---|---|
| `-c` | 显示出现次数 |
| `-d` | 只显示重复行 |
| `-u` | 只显示不重复行 |

```bash
sort file.txt | uniq
sort access.log | uniq -c | sort -nr           # 统计 Top 行
```

#### `cut` — 切列
| 参数 | 含义 |
|---|---|
| `-d` | 分隔符 |
| `-f N` | 第 N 列 |
| `-c N-M` | 第 N 到 M 个字符 |

```bash
cut -d: -f1 /etc/passwd                        # 用户名列表
cut -d, -f1,3 data.csv                         # CSV 第 1、3 列
echo "abcdef" | cut -c2-4                      # bcd
```

#### `tr` — 字符替换/删除
```bash
echo "hello" | tr 'a-z' 'A-Z'                  # HELLO
echo "a,b,c" | tr ',' '\n'                     # 一行一个
cat file.txt | tr -d '\r'                      # 删除 Windows 换行符
cat file.txt | tr -s ' '                       # 多空格压缩为一个
```

#### `diff` — 比较文件
```bash
diff a.txt b.txt
diff -u a.txt b.txt                            # 统一格式(常用)
diff -r dir1/ dir2/                            # 递归比较目录
diff --color=auto a.txt b.txt                  # 彩色输出
```

---

### 3.6 压缩与归档

#### `tar` — 打包/解包(最常用)
| 参数 | 含义 |
|---|---|
| `-c` | 创建归档 |
| `-x` | 解开 |
| `-t` | 列出内容(不解开) |
| `-v` | 显示过程 |
| `-f FILE` | 指定文件名(必须在最后) |
| `-z` | 用 gzip(`.tar.gz`) |
| `-j` | 用 bzip2(`.tar.bz2`) |
| `-J` | 用 xz(`.tar.xz`) |
| `-C DIR` | 切换到目录再操作 |
| `--exclude=PAT` | 排除匹配项 |

```bash
# 打包
tar -czvf archive.tar.gz dir/                  # 打包并 gzip
tar -czvf bak.tar.gz --exclude='*.log' dir/    # 排除 .log

# 解包
tar -xzvf archive.tar.gz                       # 解到当前目录
tar -xzvf archive.tar.gz -C /tmp/              # 解到指定目录

# 查看
tar -tzvf archive.tar.gz                       # 不解开,看内容
```

记忆口诀:打包 `czvf`,解包 `xzvf`,看内容 `tzvf`。

#### `gzip` / `gunzip`
```bash
gzip file.txt            # 压缩,生成 file.txt.gz,删原文件
gzip -k file.txt         # 保留原文件
gunzip file.txt.gz       # 解压
zcat file.txt.gz         # 不解压直接看内容
```

#### `zip` / `unzip`
```bash
zip -r archive.zip dir/
unzip archive.zip
unzip -l archive.zip                           # 只看内容,不解压
unzip archive.zip -d /tmp/                     # 解到指定目录
```

---

### 3.7 进程与作业

#### `ps` — 进程快照
```bash
ps aux                                          # BSD 风格,看所有进程
ps -ef                                          # System V 风格
ps aux | grep nginx                             # 配合 grep 查特定进程
ps -u liuxiaolong                               # 某用户的进程
ps --forest                                     # 树状显示
```

#### `top` / `htop` — 实时进程监控
```bash
top
```
**交互按键**:`P` CPU 排序,`M` 内存排序,`k` 杀进程,`q` 退出,`1` 显示每 CPU。

`htop` 更友好(可能要 `sudo apt install htop`)。

#### `kill` / `killall`
```bash
kill 1234                                       # 默认 SIGTERM(15),优雅退出
kill -9 1234                                    # SIGKILL(9),强杀
kill -l                                         # 列出所有信号
killall nginx                                   # 按名字杀
killall -9 -u liuxiaolong                       # 杀某用户所有进程
pkill -f "python myscript"                      # 按命令行匹配
```

常用信号:
- `1` SIGHUP — 重新加载配置
- `9` SIGKILL — 强杀(无法捕获)
- `15` SIGTERM — 优雅退出(默认)
- `2` SIGINT — 等同 Ctrl+C

#### 后台任务
```bash
command &                                       # 后台执行
jobs                                            # 看当前 shell 的后台任务
fg %1                                           # 把任务 1 调回前台
bg %1                                           # 让暂停的任务在后台继续
nohup command &                                 # 退出 shell 后仍运行,输出到 nohup.out
disown -h %1                                    # 已运行的任务从 jobs 列表移除
```

#### `screen` / `tmux` — 持久会话(SSH 必备)
```bash
# tmux(推荐)
tmux                                            # 新建会话
tmux new -s work                                # 命名会话
tmux ls                                         # 列出会话
tmux attach -t work                             # 重连
# 会话内快捷键:Ctrl+b 然后:
#   d  分离
#   c  新窗口
#   "  水平分屏
#   %  垂直分屏
#   方向键 在分屏间切换
```

---

### 3.8 系统信息

#### `df` — 磁盘空间
```bash
df -h                                           # 人类可读(必加)
df -hT                                          # 显示文件系统类型
df -i                                           # inode 使用情况
```

#### `du` — 目录占用
```bash
du -sh dir/                                     # 总大小,人类可读
du -sh *                                        # 当前目录每项的大小
du -h --max-depth=1                             # 只展开一层
du -ah | sort -hr | head -20                    # 找最大的 20 个文件
```

#### `free` — 内存
```bash
free -h                                         # 人类可读
free -m                                         # MB
free -s 2                                       # 每 2 秒刷新
```

#### 其他
```bash
uname -a                                        # 内核+主机+架构
uptime                                          # 运行时间+负载
who                                             # 谁登录了
w                                               # 谁登录+在做什么
last                                            # 登录历史
lscpu                                           # CPU 信息
lsblk                                           # 块设备(磁盘分区)
hostname                                        # 主机名
date                                            # 当前时间
```

---

### 3.9 网络

#### `ssh` — 远程登录
| 参数 | 含义 |
|---|---|
| `-p PORT` | 指定端口(默认 22) |
| `-i KEY` | 指定私钥 |
| `-L L:H:R` | 本地端口转发 |
| `-R R:H:L` | 远程端口转发 |
| `-N` | 只做端口转发,不开 shell |
| `-f` | 后台运行 |
| `-X` | 转发 X11 图形 |
| `-v` | 调试输出 |

```bash
ssh user@host
ssh -p 2222 user@host
ssh -i ~/.ssh/mykey user@host
ssh -L 8080:localhost:80 user@host              # 把远程 80 映射到本地 8080
```

**SSH 配置**(`~/.ssh/config`):
```
Host eda
  HostName 192.168.1.10
  User liuxiaolong
  Port 22
  IdentityFile ~/.ssh/id_rsa
```
之后只需 `ssh eda`。

#### `scp` — 文件传输(基于 SSH)
```bash
scp local.txt user@host:/remote/path/           # 上传
scp user@host:/remote/file ./                   # 下载
scp -r dir/ user@host:/path/                    # 递归
scp -P 2222 ...                                 # 指定端口(注意大写 P)
```

#### `rsync` — 增量同步(优于 scp)
| 参数 | 含义 |
|---|---|
| `-a` | 归档模式(保留权限/时间/链接等) |
| `-v` | 显示过程 |
| `-z` | 压缩传输 |
| `-h` | 人类可读 |
| `-P` | 显示进度 + 断点续传 |
| `--delete` | 删除目标端多余的文件(危险!) |
| `--exclude=PAT` | 排除 |
| `-n` | dry-run,只看不做 |

```bash
rsync -avzhP src/ user@host:/dest/              # 上传(注意末尾 / 含义)
rsync -avzhP user@host:/src/ ./dest/            # 下载
rsync -avn src/ dest/                           # 先模拟看会做什么
```

> 末尾斜杠区别:`src/` 表示同步内容,`src` 表示同步整个目录。

#### `curl` / `wget`
```bash
curl URL                                         # 输出到 stdout
curl -O URL                                      # 保存为远程文件名
curl -o file URL                                 # 保存为指定名
curl -L URL                                      # 跟随重定向
curl -I URL                                      # 只看响应头
curl -X POST -d 'a=1' URL                        # POST
curl -H "Authorization: Bearer xxx" URL          # 加请求头
curl -u user:pass URL                            # 基本认证

wget URL                                         # 直接下载
wget -c URL                                      # 断点续传
wget -r URL                                      # 递归(下整站)
wget -O file URL                                 # 指定输出名
```

#### 网络诊断
```bash
ping host                                        # 测连通
ping -c 4 host                                   # 只 4 次
traceroute host                                  # 路由路径

ss -tunlp                                        # 看监听端口(替代 netstat)
ss -tn                                           # TCP 连接

ip addr                                          # 看网卡 IP(替代 ifconfig)
ip route                                         # 路由表

dig example.com                                  # DNS 查询
nslookup example.com                             # DNS 查询
```

---

### 3.10 用户与权限

```bash
sudo command                                     # 以 root 执行
sudo -i                                          # 切到 root 交互 shell
sudo -u user command                             # 以 user 身份执行

su -                                             # 切到 root(需 root 密码)
su - user                                        # 切到 user

passwd                                           # 改自己密码
sudo passwd user                                 # 改别人密码

whoami                                           # 当前用户
id                                               # uid/gid/groups
id user                                          # 看别人

groups                                           # 当前用户所在组
groups user

# 用户管理(需 sudo)
sudo useradd -m -s /bin/bash newuser             # 创建用户(带 home,bash shell)
sudo passwd newuser
sudo usermod -aG sudo newuser                    # 加入 sudo 组
sudo userdel -r olduser                          # 删除并删 home
```

---

### 3.11 包管理 (Ubuntu/Debian)

```bash
sudo apt update                                  # 更新软件源
sudo apt upgrade                                 # 升级已安装包
sudo apt full-upgrade                            # 升级 + 处理依赖变化

sudo apt install pkg                             # 安装
sudo apt install pkg1 pkg2                       # 同时多个
sudo apt remove pkg                              # 卸载(留配置)
sudo apt purge pkg                               # 卸载并删配置
sudo apt autoremove                              # 清理无用依赖

apt search keyword                               # 搜索包
apt show pkg                                     # 看包信息
apt list --installed                             # 已安装包
apt list --upgradable                            # 可升级包

# dpkg 低层
dpkg -l                                          # 列出所有已安装
dpkg -l | grep pkg
dpkg -L pkg                                      # pkg 安装的所有文件
dpkg -S /path/file                               # 文件属于哪个包
sudo dpkg -i package.deb                         # 安装本地 deb
```

---

### 3.12 重定向 / 管道 / 通配符

```bash
cmd > file        # 标准输出覆盖写入
cmd >> file       # 追加
cmd 2> file       # 标准错误重定向
cmd > file 2>&1   # stdout 和 stderr 都到 file
cmd &> file       # 同上(bash 简写)
cmd < file        # 文件作为 stdin
cmd1 | cmd2       # cmd1 的输出作为 cmd2 的输入
cmd << EOF        # heredoc
text
EOF

cmd1 && cmd2      # cmd1 成功才执行 cmd2
cmd1 || cmd2      # cmd1 失败才执行 cmd2
cmd1 ; cmd2       # 顺序执行,无论成败
cmd &             # 后台执行
```

**通配符**:
```bash
*                 # 任意字符(0+)
?                 # 单个字符
[abc]             # a/b/c 任一
[a-z]             # a-z 任一
[!abc]            # 不是 a/b/c
{a,b,c}           # brace 展开:a 或 b 或 c
{1..10}           # 1 2 3 ... 10
~                 # home 目录
~user             # user 的 home
```

---

### 3.13 环境变量

```bash
echo $HOME                                       # 查看变量
echo $PATH | tr ':' '\n'                         # 美化看 PATH
env                                              # 所有环境变量
export VAR=value                                 # 设置(子进程可见)
VAR=value                                        # 设置(仅当前 shell)
unset VAR                                        # 删除

# 永久生效:加到 ~/.bashrc 或 ~/.profile
echo 'export EDITOR=vim' >> ~/.bashrc
source ~/.bashrc                                 # 重新加载(不用退出登录)
```

**几个重要文件加载顺序**:
- `~/.bash_profile` → 登录 shell 执行(只一次)
- `~/.bashrc` → 交互式非登录 shell 执行(开 tab/新终端)
- `~/.profile` → 登录 shell 执行(没有 .bash_profile 时)

经验法则:Ubuntu 默认 `.profile` 会调用 `.bashrc`,大多数配置写到 `~/.bashrc` 就够了。

---

### 3.14 历史与快捷键

#### `history`
```bash
history                                          # 历史命令
history 20                                       # 最近 20 条
!!                                               # 上一条命令
!100                                             # 第 100 条
!ssh                                             # 最近以 ssh 开头的
!$                                               # 上一条命令的最后一个参数
sudo !!                                          # 用 sudo 重跑上一条
```

#### Bash 快捷键
| 快捷键 | 作用 |
|---|---|
| `Ctrl+A` | 跳到行首 |
| `Ctrl+E` | 跳到行尾 |
| `Ctrl+U` | 删除光标到行首 |
| `Ctrl+K` | 删除光标到行尾 |
| `Ctrl+W` | 删除前一个单词 |
| `Ctrl+L` | 清屏(等同 `clear`) |
| `Ctrl+R` | **反向搜索历史**(高频神技) |
| `Ctrl+C` | 中断当前命令 |
| `Ctrl+D` | 退出 shell / EOF |
| `Ctrl+Z` | 暂停当前进程(用 `fg` 恢复) |
| `Tab` | 自动补全(双击 Tab 列出候选) |
| `Alt+.` | 插入上一条命令的最后一个参数 |

---

## 附录:本次配置一次性命令包

复制粘贴一气呵成:

```bash
# 1. 创建工作目录
mkdir -p ~/work/{projects,playground,scripts,notes,tools,tmp}

# 2. 配置 npm 用户级前缀(可选,推荐)
mkdir -p ~/.npm-global
npm config set prefix ~/.npm-global

# 3. 配置 .bashrc(登录自动 cd + 两个 PATH)
cat >> ~/.bashrc <<'EOF'

# ===== 自定义配置 =====
cd ~/work
export PATH=~/work/scripts:$PATH
export PATH=~/.npm-global/bin:$PATH
EOF

# 4. 立即生效
source ~/.bashrc

# 5. 安装 Claude Code(用户级,无需 sudo)
npm install -g @anthropic-ai/claude-code

# 6. 验证
which claude
claude --version
```

---

## 速查卡(打印贴墙用)

```
查找:           find . -name "*.py"    grep -rn "TODO" src/
看大文件:       less file               tail -f log
权限:           chmod 755 file          chown user:group file
压缩:           tar -czvf x.tar.gz dir/ tar -xzvf x.tar.gz
传输:           rsync -avzhP src/ host:/dst/
进程:           ps aux | grep x         kill -9 PID         tmux
磁盘:           df -h                   du -sh *            free -h
网络:           ss -tunlp               curl -L URL
历史:           Ctrl+R                  !!  !$  sudo !!
```

---

> 文档生成日期:2026-05-08
> 维护建议:把本文件放到 `~/work/notes/server-guide.md`,以后随用随查随补充
