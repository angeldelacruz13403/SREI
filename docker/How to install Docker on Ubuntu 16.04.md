# How to install Docker on Ubuntu 16.04

To install Docker on Ubuntu 16.04, follow these steps:

1. Update the package index:

```
sudo apt-get update
```

2. Install required packages to allow apt to use HTTPS:

```
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
```

3. Add Docker's official GPG key:

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

4. Add the Docker repository:

```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

5. Update the package index again:

```
sudo apt-get update
```

6. Install Docker CE:

```
sudo apt-get install docker-ce
```

7. Verify that Docker is installed correctly:

```
sudo docker run hello-world
```

8. (Optional) Add your user to the docker group to run Docker without sudo:

```
sudo usermod -aG docker ${USER}
```

Log out and back in for this change to take effect[^1][^2][^3].

After completing these steps, Docker should be successfully installed on your Ubuntu 16.04 system.

<div style="text-align: center">⁂</div>

[^1]: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04

[^2]: https://onthedock.github.io/post/170110-instala-docker-en-ubuntu-server-16.04/

[^3]: https://www.e2enetworks.com/blog/how-to-install-and-use-docker-on-ubuntu-16-04

[^4]: https://devops.stackexchange.com/questions/15544/how-to-create-an-ubuntu-image-for-16-04

[^5]: https://github.com/fmidev/smartmet-server/blob/master/docs/Setting-up-Docker-and-Docker-Compose-(Ubuntu-16.04-and-18.04.1).md

[^6]: https://docs.docker.com/desktop/setup/install/linux/ubuntu/

[^7]: https://askubuntu.com/questions/938700/how-do-i-install-docker-on-ubuntu-16-04-lts

[^8]: https://docs.docker.com/engine/install/ubuntu/

