# zsh安装文档

## zsh安装
```
sudo apt install zsh
```

## 切换默认shell
```
chsh -s /bin/zsh
```

## 安装oh my zsh
```
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh

ln -s ~/Desktop/sys-config/.zshrc ~/.zshrc
```

## 安装autojump
```
wget https://github.com/downloads/joelthelion/autojump/autojump_v21.1.2.tar.gz
```
解压，运行其中`./install.sh`

完。