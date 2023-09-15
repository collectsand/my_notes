## 使用 ___llvm.sh___ 安装
参考 https://apt.llvm.org/
```
wget https://apt.llvm.org/llvm.sh
chmod +x llvm.sh
sudo ./llvm.sh <version> #输入版本号,比如16
sudo apt-get update
sudo apt-get upgrade
```


## 使用编译好的二进制文件
去 https://github.com/llvm/llvm-project 找一个拥有编译好的二进制文件的版本(并不是每个版本都支持ubuntu20.04,有它的二进制文件)