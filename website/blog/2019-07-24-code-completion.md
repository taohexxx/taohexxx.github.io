---
layout: post
title: "Code Completion"
description: ""
category: linux
tags: [linux, vscode, neovim, vim]
---
{% include JB/setup %}

## macOS配置ccls代码补全环境

### 下载项目代码

macOS配置本地补全环境需要clone所有依赖到的项目文件到本地，对于特别庞大的目录，强烈建议忽略掉lfs的文件。命令如下：

```sh
GIT_LFS_SKIP_SMUDGE=1 git clone SERVER-REPOSITORY
```

### 安装ccls服务器端

```sh
# 安装ccls作为lsp服务器端
$ brew install llvm ccls

# 安装代码所对应的gcc版本用于生成索引
$ brew install gcc@4.9

# 查看头文件安装目录
$ brew info gcc@4.9
gcc@4.9: stable 4.9.4 (bottled)
The GNU Compiler Collection
https://gcc.gnu.org/
/usr/local/Cellar/gcc@4.9/4.9.4_1 (1,182 files, 215.3MB) *
  Poured from bottle on 2019-07-24 at 10:52:12
```

### 初步生成`compile_commands.json`文件

```sh
$ cd ~/QQMail/xxx/yyy
$ patchbuild build --svn-update :zzz --bazel-genfiles
14:45:42 PatchBuild(warning): The bazel genfiles are saved in $HOME/QQMail/bazel-genfiles
```

由于patchbuild的server环境的gcc 4路径与macOS本地不一致，需要确定本地gcc 4所在目录并修改`~/QQMail/bazel-genfiles/compile_commands.json`。例如把`-I/usr/local/include/c++/4.8.2 -I/usr/local/include/c++/4.8.2/x86_64-unknown-linux-gnu -I/usr/local/include/c++/4.8.2/backward`替换成`-I/usr/local/Cellar/gcc@4.9/4.9.4_1/include/c++/4.9.4 -I/usr/local/Cellar/gcc@4.9/4.9.4_1/include/c++/4.9.4/x86_64-apple-darwin17.3.0 -I/usr/local/Cellar/gcc@4.9/4.9.4_1/include/c++/4.9.4/backward`。然后保存到`~/QQMail/compile_commands_gcc.json`。以下是vim全局替换命令：

```vim
:%s/-I\/usr\/local\/include\/c++\/4.8.2 -I\/usr\/local\/include\/c++\/4.8.2\/x86_64-unknown-linux-gnu -I\/usr\/local\/include\/c++\/4.8.2\/backward/-I\/usr\/local\/Cellar\/gcc@4.9\/4.9.4_1\/include\/c++\/4.9.4 -I
\/usr\/local\/Cellar\/gcc@4.9\/4.9.4_1\/include\/c++\/4.9.4\/x86_64-apple-darwin17.3.0 -I\/usr\/local\/Cellar\/gcc@4.9\/4.9.4_1\/include\/c++\/4.9.4\/backward/g
```

### 测试单一文件能否重新正确生成索引

根据修改后的`compile_commands_gcc.json`重新生成索引。下面语句只重新生成`iTLVFastReader.cpp`的索引：

```sh
cd ~/QQMail
compile_commands ./compile_commands_gcc.json --workspace=$HOME/QQMail --genfiles-dir-path=$HOME/QQMail/bazel-genfiles/local_linux-fastbuild/genfiles --test-run --file-list=iTLVFastReader.cpp
```

如果这步出现错误，而又没有输出错误原因，修改工具源代码`~/QQMail/mmtools/blade/get_compile_commands.py`：

```python
class RunTestContext(object):
  def __init__(self, print_cmd = False):
     ...
    # 找到self.print_cmd = print_cmd这一行，改成始终输出错误
    self.print_cmd = True
```

再次执行上面的`--test-run`命令，工具会输出gcc命令，复制gcc命令自己执行一次，可以看到具体原因。

### 重新生成所有索引

```sh
cd ~/QQMail
compile_commands ./compile_commands_gcc.json --workspace=$HOME/QQMail --genfiles-dir-path=$HOME/QQMail/bazel-genfiles/local_linux-fastbuild/genfiles --test-run
```

如果成功的话，正式生成

```sh
cd ~/QQMail
compile_commands ./compile_commands_gcc.json --workspace=$HOME/QQMail --genfiles-dir-path=$HOME/QQMail/bazel-genfiles/local_linux-fastbuild/genfiles
```

### 安装客户端

客户端可以使用Neovim或者MacVim，搭配[coc.nvim](https://github.com/neoclide/coc.nvim)插件。下面是使用插件管理器[Dein.vim](https://github.com/Shougo/dein.vim)进行安装的示例。

加入[coc.nvim](https://github.com/neoclide/coc.nvim)插件

```vim
call dein#begin('~/.cache/dein')
```

Use release branch (recommended):

```vim
call dein#add('neoclide/coc.nvim', {'merged': 0, 'rev': 'release'})
```

Build from source code:

```vim
call dein#add('neoclide/coc.nvim', {'merged': 0, 'build': 'yarn install --frozen-lockfile'})
```

```vim
call dein#end()
```

Note: when `'merged': 0` not present, coc.nvim will be unable to start.

Note: depends on your network and CPU, it might take a long time for your first build.

If you have trouble with compiling the source code when using dein, try command:

```sh
cd ~/.cache/dein/repos/github.com/neoclide/coc.nvim
git clean -xfd
yarn install --frozen-lockfile
```

<https://github.com/neoclide/coc.nvim/wiki/Install-coc.nvim>

### 安装插件

#### CocInstall安装

vim内运行

```vim
CocInstall coc-snippets coc-json coc-tsserver coc-html coc-css coc-java coc-r-lsp coc-yaml coc-python coc-highlight coc-lists coc-git coc-yank coc-svg coc-vimlsp coc-xml
CocList extensions
CocList snippets
```

在线机器装好后，可复制`~/.config/coc`到离线机器。离线机器要装有java，否则每次启动vim会提示找不到java，要按一下回车才能继续。

#### vim插件方式安装

以coc-snippets为例

```vim
call dein#add('neoclide/coc.nvim', {'merged': 0, 'build': 'yarn install --frozen-lockfile'})
call dein#add('neoclide/coc-snippets', {'depends': 'neoclide/coc.nvim', 'merged': 0, 'build': 'yarn install --frozen-lockfile'})
```

### 快捷键配置

```vim
" gotos
nmap <silent> <Leader>cd <Plug>(coc-definition)
nmap <silent> <Leader>cy <Plug>(coc-type-definition)
nmap <silent> <Leader>ci <Plug>(coc-implementation)
nmap <silent> <Leader>cr <Plug>(coc-references)

" rename current word
nmap <Leader>cn <Plug>(coc-rename)

" format selected region
xmap <Leader>cf <Plug>(coc-format-selected)
nmap <Leader>cf <Plug>(coc-format-selected)
nmap <silent> <Leader>cm <Plug>(coc-format)

" show documentation in preview window
nnoremap <silent> <SID>show-doc :call <SID>ShowDoc()<CR>
nmap <Leader>ch <SID>show-doc
function! <SID>ShowDoc()
  if (index(['vim', 'help'], &filetype) >= 0)
    execute 'h '.expand('<cword>')
  else
    call CocAction('doHover')
  endif
endfunction
```
