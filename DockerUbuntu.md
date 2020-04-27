# Install Docker on Ubuntu

1. Remove existing installations
  ```
  sudo apt-get remove docker docker-engine docker.io containerd runc
  ```
2. Update the apt package index and install packages to allow apt to use a repository over HTTPS:
  ```
  sudo apt update && sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common -y
  ```
3. Add Docker’s official GPG key:
  ```
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
  ```
4. Use the following command to set up the stable repository
  ```
  sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
  ```
5. Update the apt package index, and install the latest version of Docker Engine and containerd,
  ```
  sudo apt-get install docker-ce docker-ce-cli containerd.io -y
  ```
6. If you want to avoid typing sudo whenever you run the docker command, add your username to the docker group:
  ```
  sudo usermod -aG docker ${USER}
  ```
7. To apply the new group membership, reboot or logout and log back in 
  ```
  logout
  ```
  
## References

1. [Official Docs](https://docs.docker.com/engine/install/ubuntu/)
2. [DigitalOcean's tutorial](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04)
