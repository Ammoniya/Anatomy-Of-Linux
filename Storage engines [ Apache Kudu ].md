<h2> Apache KUDU </h2> <img src="https://upload.wikimedia.org/wikipedia/en/thumb/9/94/Apache_Kudu_Logo.svg/1200px-Apache_Kudu_Logo.svg.png" alt="drawing" width="200"/>

### installation kudu <br>

```
#!/bin/bash
sudo apt-get update
sudo apt-get install -y software-properties-common
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt update
sudo apt install g++-7 -y
sudo apt update
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 60 \
                         --slave /usr/bin/g++ g++ /usr/bin/g++-7 
sudo update-alternatives --config gcc
gcc --version
g++ --version
sudo apt-get -y install autoconf automake curl flex g++ gcc gdb git \
  krb5-admin-server krb5-kdc krb5-user libkrb5-dev libsasl2-dev libsasl2-modules \
  libsasl2-modules-gssapi-mit libssl-dev libtool lsb-release make ntp \
  openjdk-8-jdk openssl patch pkg-config python rsync unzip vim-common
git clone https://github.com/apache/kudu
cd kudu
thirdparty/build-if-necessary.sh
mkdir -p build/release
cd build/release
../../thirdparty/installed/common/bin/cmake \
  -DCMAKE_BUILD_TYPE=release ../..
make -j4
```

![image](https://user-images.githubusercontent.com/20130001/102718253-394e4a00-430d-11eb-8ec5-09ccd79fea0b.png)
