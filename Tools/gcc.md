# gcc 최신 버전 설치하는 법 (Ubuntu)
> Ubuntu Linux 환경에서 gcc 최신버전을 설칳하는 방법 및 버전 변경 방법에 대해 알아본다.

## gcc 최신버전 설치 
~~~
sudo apt-get update
sudo apt-get install build-essential software-properties-common -y
sudo add-apt-repository ppa:ubuntu-toolchain-r/test -y
sudo apt-get update
sudo apt-get install gcc-snapshot -y 
sudo apt-get update 
sudo apt-get install gcc-9 g++-9 -y

~~~

* gcc-9 대신 다른 버전을 설치할 수 있다. [참고](https://gist.github.com/application2000/73fd6f4bf1be6600a2cf9f56315a2d91)

## gcc 버전 변경방법

~~~
sudo rm /usr/bin/gcc
sudo rm /usr/bin/g++
sudo ln -s /usr/bin/gcc-new /usr/bin/gcc
sudo ln -s /usr/bin/g++-new /usr/bin/g++

# Check symbolic links
sudo ls -la /usr/bin/ | grep gcc
# Check gcc version
gcc -v
~~~

* gcc-new, g++-new는 새로운 버전의 gcc/g++를 의미한다. [참고](https://www.quora.com/How-do-I-downgrade-gcc-in-ubuntu-14-04-4-8-4-to-4-7-*)

