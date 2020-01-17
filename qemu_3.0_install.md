# Install QEMU 3.0 on Ubuntu 18.04.3 LTS

1. Remove all existing `qemu` packages 

```console
ubuntu@ip-172-31-18-86$ sudo apt-get purge "qemu*"
ubuntu@ip-172-31-18-86$ sudo apt autoremove
```

2. Get build dependencies for `qemu`

```console
ubuntu@ip-172-31-18-86$ sudo sed -i '/deb-src/s/^# //' /etc/apt/sources.list
ubuntu@ip-172-31-18-86$ sudo apt update
ubuntu@ip-172-31-18-86$ sudo apt build-dep qemu
```

3. Build it

```console
ubuntu@ip-172-31-18-86$ wget https://download.qemu.org/qemu-3.0.0.tar.xz
ubuntu@ip-172-31-18-86$ tar xf qemu-3.0.0.tar.xz && rm qemu-3.0.0.tar.xz
ubuntu@ip-172-31-18-86$ cd qemu-3.0.0 && ./configure && make -j$(nproc)
```

4. Create appropriate symlinks
```console
ubuntu@ip-172-31-18-86$ find . -name 'qemu*' -type f -executable
ubuntu@ip-172-31-18-86$ find . -name 'qemu-system-arm' -type f -executable
./arm-softmmu/qemu-system-arm
ubuntu@ip-172-31-18-86$ ln -s /home/ubuntu/source/mipsel_awsiot/qemu-3.0.0/arm-softmmu/qemu-system-arm ../venv/bin/qemu-system-arm
```


