# Introduce
> WSL(Windows subsystem for Linux)是微软在Win10版本之后开发的在Windows系统上运行Linux内核的功能。好处是不会产生传统虚拟机和双系统启动的开销。
- 这里明确一下，WSL只是Windows系统中的一个子系统，相当于是Windows系统提供的一个Linux系统入口，其内存占用和性能依然取决于Windows系统（安装完成后，可以在Windows系统的C盘中找到WSL的身影，同样的，在WSL的虚拟机中也可以访问Windows系统的文件，这样也就避免了新手使用虚拟机系统时设置不当会造成内存泄露的问题。）
# Install
- 打开Windows终端，输入
  ```
  wsl --install
  ```
  进行安装
> 需要VPN
- 安装完成后，需要配置user name 和 passwd，这些按照指引做就可以。
- 具体内容可以看微软官方文档[文档在这里]("https://learn.microsoft.com/zh-cn/windows/wsl/")
> 在文档中，还可以根据自己的需求配置VSCode、Git等内容，并且结合自己的开发需求，在WSL系统中添加需要的模块。

# Config
## VSCode
- 在安装完成的WSL内核（默认Ubuntu系统）中输入
```
code .
```
系统就会自己配置适用于WSL的VSCode。
- 在VSCode中，我们要做的是：
- 安装Remote development（在Extension中搜索安装即可）
- 在VSCode中，使用*Crtl + Shift + P*调用命令菜单，输入*WSL*，然后选择*WSL: Connect to WSL*就可以在左下角看到连接状态。
- 使用*Crtl + Shift + `*调出VSCode的终端，我们就可以看到这时的终端已经变成了Linux的形状。
> 如果想写一个程序测试一下是不是真的Linux系统，我们只需要
```
vim hello.c

i

#include<stdio.h>
int main(void)
{
    printf("Hello world!\n");
}

ESC

:wq

sudo apt install make

make hello

./hello
```
> 如果一切顺利的话，你会在终端看到美妙的**Hello world!**字样。  
> 此外，如果使用GUN打开hello.c文件，发现有报错，说明在WSL中没有配置C/C++的头文件所在的位置。这时，我们只需要在终端中输入
```
gcc -v -E -x c++ -

```
> 然后复制输出结果中的最后几行路径，粘贴在C/C++ Extension 文件中的includePath下，注意格式。给你们can can word
```
"includePath": [
                "${workspaceFolder}/**",
                "/usr/include/c++/11",
                "/usr/include/x86_64-linux-gnu/c++/11",
                "/usr/include/c++/11/backward",
                "/usr/lib/gcc/x86_64-linux-gnu/11/include",
                "/usr/local/include",
                "/usr/include/x86_64-linux-gnu",
                "/usr/include"
                
            ],
```

## Make it better

### zsh 
- 可以配合插件在Linux中实现命令高亮，命令补全等功能
```
sudo apt install zsh -y

# 安装 oh-my-zsh
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh || true

# 安装命令补全和高亮插件
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/plugins/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/plugins/zsh-autosuggestions
sed -i 's/plugins=(git)/plugins=(git zsh-autosuggestions zsh-syntax-highlighting)/g' ~/.zshrc

# 将 zsh 设置为默认的shell
chsh -s /bin/zsh
```
