1. Check out appropriate version of qemu: `git clone -b v2.12.1 https://github.com/qemu/qemu.git`
2. Try to `./configure && make`. This will fail with some `string...` error on new `gcc`
3. Need to install old `gcc`
  1. Update apt sources: Add `deb http://old.kali.org/kali sana main non-free contrib` to `/etc/apt/sources.list`
  2. `apt update`
4. `apt-cache madison gcc`
  ```
  gcc |  4:9.2.1-3 | http://http.kali.org/kali kali-rolling/main amd64 Packages
  gcc |  4:4.9.2-2 | http://old.kali.org/kali sana/main amd64 Packages
  ```
5. `apt install gcc=4:4.9.2-2` 
6. `cd qemu`
7. `./configure`
8. `make && make install`
9. `cd ../ && rm -rf qemu`
