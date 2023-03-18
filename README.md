# Explore Kernel Source Code with Sublime

It is a guide for setting up linux kernel source code exploring environment with Sublime.

The guide is based on Ubuntu, but you can still use it as a reference for Windows or MacOS, it will still work.

## 1. Install Tools

### Install Sublime Text

<https://www.sublimetext.com/download>

### Install Sublime Merge

<https://www.sublimemerge.com/download>

### Install Ctags

`sudo apt install ctags`

### Install Cscope

`sudo apt install ctags`

## 2. Install Sublime Plugins

Install the following plugins in sublime package control:

- Ctags
- CscopeSublime
- Git blame

Other Plugins：

- BetterFindBuffer (cooperate with cscope results view, support click jump)
- Outline (sidebar function outline)
- Simple ifdef (highlight #ifdef)
- Sublimerge Pro (diff tool)
- Devicetree DTS Highlighting (.dts/.dtsi support)
- View Bootkmarks (view all bookmarks)

For better experience, you need apply the patches for the plugins:

- BetterFindBuffer: [sublime-betterfindbuffer.patch](https://github.com/kernel-cyrus/linux-sublime/blob/master/sublime-betterfindbuffer.patch)
- CscopeSublime: [sublime-cscope.patch](https://github.com/kernel-cyrus/linux-sublime/blob/master/sublime-cscope.patch)

## 3. Generate ctags and cscope

For linux kernel, after your kernel is built, you can generate .tag with:

```
make tags cscope
mv tags .tags
```

For android common kernel, you need copy all .o to source folder

```
cp -r out/android.../common ./
make ARCH=arm64 COMPILED_SOURCE=1 tags cscope
make clean,mrproper
mv tags .tags
```

## 4. Configure Sublime Settings

### Modify Project Settings

```
{
    "settings":{
        "font_face": "Consolas",                # Set mono font
        "font_size": 11,                        # Set font size
        "tab_size": 8,                          # Set tab size for kernel code
        "translate_tabs_to_spaces": false,      # Set tab for kernel code
        "index_files": false,                   # Disable auto indexing (we use ctags and cscopes)
        "auto_complete": false,                 # Disable auto completion
        "auto_match_enabled": false             # Disable auto match
        "tab_completion": false,                # Disable tab completion
        "detect_indentation": false,            # Disable indentation auto detect
    },
    "folders":
    [
        {
            "path": "",                         # Set kernel source code path
            "folder_exclude_patterns":          # Only keep aarch64 files
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
                "common/lib",
                "common/LICENSES",
                "common/net",
                "common/samples",
                "common/scripts",
                "common/security",
                "common/sound",
                "common/tools",
                "common/usr",
                "common/certs",
            ],
            "file_include_patterns":            # Only keep source files
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
```

### Modify Key Bindings

```
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
```

### Modify Other Plugin Settings

CscopeSublime: `"prompt_before_searching": false`

## 5. Usages

### Navigation

`alt + 1`: Jump to definition

`alt + 2`: Look up functions calling this function

`alt + 3`: Jump back

`alt + 4`: Jump forward

`alt + 5`: Look up this symbol

`alt + 6`: Look up global definition

`alt + f1`: Jump to definition (sublime index)

`alt + f2`: View all bootmarks

`ctrl + \`: More cscope search commands

### Git History/Blame

`right click menu` - `Open Git Repository...`: Use Sublime Merge to open this repository

`right click menu` - `File History...`: Use Sublime Merge to open git log of this file

`right click menu` - `Line History...`: Use Sublime Merge to open git log of this line

`shift + ctrl + q`: Show git blime of this line

`shift + ctrl + c`: Show git blame of each line

### Bootkmark

`f2`: Toggle a bootkmark

`ctrl + shift + f`: Search from all files

### Compare/Diff

`right click menu` - `sublimerge...`: Compare this file to ...
