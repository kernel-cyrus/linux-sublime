# Explore Kernel Code with Sublime

It is a guide for setting up linux kernel code exploring environment with Sublime.

The guide is based on Ubuntu, but you can still use it as a reference for Windows or MacOS, it will still work.

## 1. Install Tools

### Install Sublime Text

<https://www.sublimetext.com/download>

### Install Sublime Merge

<https://www.sublimemerge.com/download>

### Install Ctags

`sudo apt install universal-ctags`

### Install Cscope

`sudo apt install cscope`

## 2. Install Sublime Plugins

Install the following plugins in sublime package control:

- Ctags
- Git blame
- Outline (sidebar function outline)
- Simple ifdef (highlight #ifdef)
- Sublimerge Pro (diff tool)
- Devicetree DTS Highlighting (.dts/.dtsi support)
- View Bootkmarks (view all bookmarks)

Install the following plugins from git clone:

```
git clone https://github.com/kernel-cyrus/fork-BetterFindBuffer.git ~/.config/sublime-text/Packages/BetterFindBuffer
git clone https://github.com/kernel-cyrus/fork-CscopeSublime.git ~/.config/sublime-text/Packages/CscopeSublime
```

The forks fix symbol jump issue and improve functions to explore the linux kernel code

## 3. Generate ctags and cscope

For linux kernel, after kernel has been built, you can generate .tag and cscope index files with:

```
make ARCH=arm64 COMPILED_SOURCE=1 tags cscope
mv tags .tags
```

For android common kernel, you need copy all .o to source folder first:

```
cp -r out/android.../common ./
make ARCH=arm64 COMPILED_SOURCE=1 tags cscope
make clean,mrproper
mv tags .tags
```

## 4. Configure Sublime Settings

### Modify Project Settings

You need to add kernel source path in the "path" field.

```
{
    "settings":{
        "font_face": "Consolas",                // Set mono font
        "font_size": 11,                        // Set font size
        "tab_size": 8,                          // Set tab size for kernel code
        "translate_tabs_to_spaces": false,      // Set tab for kernel code
        "index_files": false,                   // Disable auto indexing (we use ctags and cscopes)
        "auto_complete": false,                 // Disable auto completion
        "auto_match_enabled": false,            // Disable auto match
        "tab_completion": false,                // Disable tab completion
        "detect_indentation": false,            // Disable indentation auto detect
    },
    "folders":
    [
        {
            "path": "",                         // Set kernel source code path
            "folder_exclude_patterns":          // Only keep aarch64 files
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
            "file_include_patterns":            // Only keep source files
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

### Other Plugin Settings

CscopeSublime: `"prompt_before_searching": false`

## Start to use

### Navigation

`ctrl + p`: Search a file name

`alt + 1`: Jump to definition

`alt + 2`: Look up functions calling this function

`alt + 3`: Jump back

`alt + 4`: Jump forward

`alt + 5`: Look up this symbol

`alt + 6`: Look up global definition

`alt + f1`: Jump to definition (sublime index)

`ctrl + \`: More cscope search commands

`ctrl + r`: Search symbol inside this file

`ctrl + shift + f`: Search from all files

\* If click the search result not jump into the function, press `enter` on the result line to jump.

### Git

`right click menu` - `Open Git Repository...`: Use Sublime Merge to open this repository

`right click menu` - `File History...`: Use Sublime Merge to open git log of this file

`right click menu` - `Line History...`: Use Sublime Merge to open git log of this line

`shift + ctrl + q`: Show git blime of this line

`shift + ctrl + c`: Show git blame of each line

### Bootkmark

`f2`: Toggle a bootkmark

`alt + f2`: View all bootmarks

### Compare

`right click menu` - `sublimerge...`: Compare this file to ...
