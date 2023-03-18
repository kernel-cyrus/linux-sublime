# Explore kernel code with Sublime

Sublime kernel代码阅读环境，环境搭建在ubuntu20.04上

## 环境安装

### 1、安装Sublime

从Sublime官网下载并安装Sublime，或者通过apt install安装：

```
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo apt install apt-transport-https
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt update
sudo apt install sublime-text
```

### 2、安装ctag

```
apt install ctags
```

### 3、安装cscope

```
apt install ctags
```

## 安装插件

The package includea all the plugins and settings.
Fix cscope jump issue.
Fix butter result buffer plugin fold issue.
Add shortcuts keys.
Just copy the package to overwrite the original folder will be all ok.

安装ctags插件
1. install ctags in package control
2. create .attrs file for parsing special attrs (for kernel, OPTIONAL)
__init
__initdata
__exitdata
__initconst
__initdata_memblock
__refdata
__attribute
__maybe_unused
__always_unused
__acquires
__releases
__deprecated
__read_mostly
__aligned
____cacheline_aligned
____cacheline_aligned_in_smp
__cacheline_aligned
__cacheline_aligned_in_smp
____cacheline_internodealigned_in_smp
__no_sanitize_address
__used
__packed
__packed2__
__must_check
__must_hold
EXPORT_SYMBOL
EXPORT_SYMBOL_GPL
ACPI_EXPORT_SYMBOL
DEFINE_TRACE
EXPORT_TRACEPOINT_SYMBOL
EXPORT_TRACEPOINT_SYMBOL_GPL
static
const
3. create .ctags mannually
make tags						# create .tags by kernel make, make after build
for android kernel, build android kernel, copy out/common to common folder, move all .o to source folder
make ARCH=arm64 COMPILED_SOURCE=1 tags
make clean,mrproper
-----------------------------
ctags -f .tags -L cscope.files -I ./.attrs	# create .tags file from cscope file list
ctags -f .tags -R .../ .../ .../			# create .tags file from paths
click folder, click "rebuild ctags"		# create .tags file from sublime
4. use navigate to definition

安装cscope插件
1. downlaod Packages.7z (I fixed the jump issue), extract to "Packages" folder
2-a. make cscope	# create cscope file from kernel make, for linux
2-b. for android kernel, build android kernel, copy out/common to common folder, move all .o to source folder
make ARCH=arm64 COMPILED_SOURCE=1 cscope
make clean,mrproper
2-c. create cscope.out mannually
find arch/arm64 kernel drivers lib block include fs mm init virt -name '*.c' -o -name '*.h' > cscope.files
cscope -i cscope.files
2-d. click folder, click "rebuild cscope"

安装git blame插件
1. package control中安装git blame
2. 在对应代码行上shift + ctrl + q即可显示该行代码对应的提交信息
3. shift + ctrl + c显示文件每行的blame信息
代码读不懂时，git blame找到当时的patch非常好用

安装sublime merge
官网安装sublime merge，可以和sublime完美结合，使用git log，git history
结合git blame，更好了解文件最初的设计和某行代码提交的原因

User Settings
"font_face": "Consolas",
"font_size": 11,
"ignored_packages":
["Vintage",],
"tab_size": 8,
"translate_tabs_to_spaces": false,
"binary_file_patterns":[],
"show_git_status":false,
"index_files": false,
"index_workers": 6
"auto_complete": false,
"auto_match_enabled": false
"tab_completion": false,
"detect_indentation": false,

Key Binding Settings
[
    { "keys": ["alt+f1"],   "command": "goto_definition" },
    { "keys": ["alt+1"],    "command": "navigate_to_definition", },
    { "keys": ["alt+2"],    "command": "cscope",  "args": {"mode": 3}},
    { "keys": ["alt+3"],    "command": "jump_back" },
    { "keys": ["alt+4"],    "command": "jump_forward" },
    { "keys": ["alt+5"],    "command": "cscope",  "args": {"mode": 0}},
    { "keys": ["alt+6"],    "command": "cscope",  "args": {"mode": 11}},
    { "keys": ["alt+f2"],   "command": "view_bookmarks"},
    { "keys": ["f2"],       "command": "toggle_bookmark"},
    { "keys": ["alt+`"],    "command": "goto_symbol_in_project" },
]

Cscope Plugin Settings
{"prompt_before_searching": false}

Index Issue
Sublime建立索引可能很慢，时间久了还可能坏掉。
如果看到CPU占用率高，且出现大量application error的日志，就是索引坏掉了。
1. 禁用索引"index_files": false
* 很多工程，适合关闭全局index，比如kernel。关闭后只会索引你代开过的文件，这就避免了符号在其他文件重复定义所导致无法唯一跳转的问题，符号跳转起来会更方便。
* 关闭索引后，ctrl+shift+r可能失效，可以重新binding到其他按键继续使用全局符号查找
2. 重建索引，删除D:\Users\50000795.ADC\AppData\Local\Sublime Text\Index所有文件
3. 在Edit Project中，添加file(folder)_include(exclude)_patterns减少索引文件量
4. 使用"index_workers": 6增加索引worker数
另外，索引建立时，可以在status bar看到进度，也可以打开help->index status查看进度。

Other Plugins：
devicetree highlight （.dts/.dtsi support）
view bookmarks（view all bookmarks）
sublimerge3（merge tool）
betterfindbuffer（cooperate with cscope results view）
outline（sidebar function outline）

Kernel Project Settings
{
    "settings":{
        "tab_size": 8,
        "translate_tabs_to_spaces": false,
        "show_git_status":false,
        "index_files": false,
    },
    "folders":
    [
        {
            "path": "/Volumes/workspace/kernel/android12-5.10/common/",
            "folder_exclude_patterns":
            [
                "common/arch/arc",
                "common/arch/arm",
                "common/arch/c6x",
                "common/arch/csky",
                "common/arch/h8300",
                "common/arch/hexagon",
                "common/arch/ia64",
                "common/arch/m68k",
                "common/arch/microblaze",
                "common/arch/mips",
                "common/arch/alpha",
                "common/arch/nds32",
                "common/arch/nios2",
                "common/arch/openrisc",
                "common/arch/parisc",
                "common/arch/powerpc",
                "common/arch/riscv",
                "common/arch/s390",
                "common/arch/sh",
                "common/arch/sparc",
                "common/arch/um",
                "common/arch/x86",
                "common/arch/xtensa",
                "common/arch/i386",
                "common/arch/x86_64",
                "common/crypto",
                "common/ipc",
                "common/lib",
                "common/LICENSES",
                "common/net",
                "common/samples",
                "common/scripts",
                "common/security",
                "common/sound",
                "common/tools",
                "common/usr",
                "common/virt",
                "common/certs",
                "common/android"
            ],
            "file_include_patterns":
            [
                "*.h",
                "*.c",
                "*.s",
                "*.S",
                "*.dts",
                "*.dtsi",
                "Kconfig",
                "Kbuild",
                "Makefile",
                "*defconfig*",
                "*.fragment",
                "*.rst",
                "*.txt",
                "*.yaml"
            ],
        }
    ],
}

