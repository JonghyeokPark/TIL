# clang 최신 버전 설치하는 법 (Ubuntu)
> Ubuntu Linux 환경에서 clang 최신버전을 설칳하는 방법 및 버전 변경 방법에 대해 알아본다.

## Prerequisite

* public key (if not existed)
~~~
ssh-keygen
~~~

* misc library
~~~
sudo apt install build-essential subversion cmake python3-dev libncurses5-dev libxml2-dev libedit-dev swig doxygen graphviz xz-utils -y
~~~

## clang 8.0 Install

~~~
sudo wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key | apt-key add - \
    && add-apt-repository "deb http://apt.llvm.org/bionic/ llvm-toolchain-bionic-8 main" \

sudo apt-get update
sudo apt-get upgrade
sudo apt-get install clang-8 libc++-8-dev libc++abi-8-dev lldb-8.0 lld-8.0
sudo ln -s /usr/bin/clang-8 /usr/bin/clang
sudo ln -s /usr/bin/clang++-8 /usr/bin/clang++

sudo apt-get install libc++abi-8-dev
~~~

## Trouble Shooting

* sudo apt-get update fail
    * `llvm-toolchain-bionic-8 main` add-apt-repository 설정 후, `sudo apt-get update` 실패하는 경우 아래와 같은 에러 메시지가 반환됨.
    ~~~
    Err:5 http://apt.llvm.org/bionic llvm-toolchain-bionic-8 InRelease   
  The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 15CF4D18AF4F7421
    ...
    ~~~
    * public key (`15CF4D18AF4F7421`)를 추가해주면된다. 
    ~~~
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 15CF4D18AF4F7421
    ~~~

* '__cxxabi_config.h' file not found 에러 
~~~
clang version (현재 버전은 8)에 맞는 libc++abi 라이브러리를 설치했는지 확인한다.
sudo apt-get install libc++abi-8-dev
~~~

## Reference
* [Building Clang8, LLVM8, libc++ and lldb on Ubuntu 18.04](https://solarianprogrammer.com/2013/01/17/building-clang-libcpp-ubuntu-linux/)

