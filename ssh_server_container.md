1. Install podman using instructions in [this gist](https://gist.github.com/ek-nath/af4cdd7144a31a14121bc9c49fec6433#file-install_podman_wsl_ub2004-sh)
2. Create a dockerfile with [this content](https://gist.github.com/ek-nath/5ab01e7bd110a3c4550f011a014d765d#file-ssh_server-dockerfile)
3. Build the image: `podman build . -t ssh_server -f ssh_server.Dockerfile`
4. Run the container: `podman run -p 2022:22 ssh_server`
5. (Optional) Check for open ports and details:
  1. `podman ps`
  2. `podman port -l`
6. ssh from another terminal:
    ```
    :: ~ » ssh test@localhost -p 2022
    test@localhost's password: 
    Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.10.16.3-microsoft-standard-WSL2 x86_64)
    To run a command as administrator (user "root"), use "sudo <command>".
    See  "man sudo_root" for details.
    ....
    test@44250f0b25c5:~$ whoami
    test
    ```
