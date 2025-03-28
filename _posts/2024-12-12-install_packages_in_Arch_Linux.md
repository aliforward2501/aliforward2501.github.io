---
title: Arch Linux安装软件包
date: 2024-12-12
---

# 简介

有官方软件仓库、非官方软件仓库（Arch社区维护）
## 使用`pacman`安装官方软件包

`sudo pacman -S packname_name`
### 其他相关命令
 
1. 搜索软件包  
`pacman -Ss package_name`  
2. 查看已安装的软件包  
`pacman -Qs packages_name`  
3. 删除软件包（保留配置文件）  
`sudo pacman -R packages_name`  
4. 删除软件包及其依赖  
`sudo pacman -Rsn packages_name`  
5. 更新系统  
`sudo pacman -Syu` 

<!--end-->
## 使用`yay`或`paru`安装AUR软件包

AUR（Arch User Repository）是Arch社区维护的非官方软件仓库，其中的软件需要用`yay`或`paru`安装
### 安装`yay`

首先需要安装`yay`
1. 先安装`git`  
`sudo pacman -S --needed base-devel git`  
`base-devel` 是编译 AUR 包所需的工具集合，如 `make`、`gcc`、`pkg-config` 等  

2. 克隆`yay`并安装  

```
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

### 使用`yay`安装AUR软件

`yay -S packages_name`
### 其他`yay`相关命令

1. 搜索AUR包  
`yay -Ss packages_name`  
2. 更新AUR软件  
`yay -Syu`

<!--end-->
### 安装`paru`

1. 安装`git`  
`sudo pacman -S --needed base-devel git`  
2. 克隆`paru`并安装  
```
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
  ```

### 使用`paru`安装软件

`paru -S packages_name`
### 其他`paru`相关命令

1. 搜索软件包  
`paru -Ss packages_name`  
2. 更新所有软件包（包括AUR）  
`paru -Syu`  
3. 仅更新AUR包  
`paru -Sua`  
4.  删除软件包  
`paru -R package_name` 
