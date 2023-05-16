---
title: "Docker Desktop on a Local Client."
seoTitle: "Local Docker Desktop: Remote Container Access"
seoDescription: "Optimize Docker Desktop on Ubuntu 22.04 LTS; enable password-less container access; improve deployment, remote access, and distribution."
datePublished: Tue May 16 2023 07:00:39 GMT+0000 (Coordinated Universal Time)
cuid: clhpxd43209m5jvnv90sb0jab
slug: docker-desktop-on-a-local-client
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1684197009190/646a2667-f467-404c-ad3d-58578abff1db.png
tags: ssh, rsa, docker-desktop, openssh-server, sshdconfig

---

## Requirements.

This lab uses the following equipment:

* The `homelab` NUC\* that runs Ubuntu Desktop 22.04 LTS, and
    
* The `workstation` PC that runs Ubuntu Desktop 22.04 LTS.
    

> NOTE: NUC\* stands for Next Unit of Computing which is nothing more than a compact, small form factor computer.

## An Introduction.

I apologise for the lack of pictures in this post, but do I really need to validate my processes with screen grabs? It's a post, not a podcast, or especially a video. Besides, [the "12 Startups in 12 Months" challenge](https://solodev.app/the-12-startups-in-12-months-challenge) has helped me to prioritise function over form. Looking good is nice. Working well is better. Working together is best. Ooh, *that* belongs on a tee shirt.

Anyway, now that I have [Docker running on my `homelab`](https://solodev.app/docker-as-a-homelab-service), it's time to install, and set up, [Docker Desktop](https://docs.docker.com/desktop/install/linux-install/) on my Ubuntu `workstation`.

## Generating, and Using, Private Keys on a Client System.

* On the `workstation` system, I start the SSH agent:
    

```plaintext
eval 'ssh-agent'
```

* I generate an RSA key pair named `/home/brian/.ssh/docker`:
    

```plaintext
ssh-keygen -b 4096
```

* I use `ssh-copy-id` to upload the `docker` public key to the remote `docker` container:
    

```plaintext
ssh-copy-id -i ~/.ssh/docker.pub -p 22 brian@192.168.188.[?]
```

> NOTE: A password is needed to login, the -p(ort) flag points to the default 22, and the "brian@192.168.188.\[?\]" user account was [created in an earlier post](https://solodev.app/docker-as-a-homelab-service#heading-adding-a-user-account-to-the-docker-container).

* I can now login to the `docker` container without a password:
    

```plaintext
ssh -p 22 brian@192.168.188.[?]
```

## Editing the "sshd\_config" File.

* Within the remotely-connected container, I open the `sshd_config` file:
    

```plaintext
sudo nano /etc/ssh/sshd_config
```

* I edit this setting and save the `sshd_config` file:
    

```plaintext
PasswordAuthentication no
```

* I test the SSH configuration:
    

```plaintext
sshd -t
```

I reboot the `docker` container:

```plaintext
sudo reboot
```

> NOTE: Running the `exit`, `sudo reboot`, or `sudo poweroff` commands will close the connection to the remote host.

## Setting Up the Docker Desktop Repo.

* On the `Workstation` system, I run the updates and upgrades:
    

```plaintext
sudo apt clean && sudo apt update && sudo apt dist-upgrade -y && sudo apt autoremove -y
```

* I install the prerequisites:
    

```plaintext
sudo apt install -y ca-certificates curl gnupg lsb-release
```

* I make a keyrings directory:
    

```plaintext
sudo mkdir -p /etc/apt/keyrings
```

* I download the Docker key to the keyrings directory:
    

```plaintext
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

* I download the Docker repository entry:
    

```plaintext
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

* I update my local repo entries:
    

```plaintext
sudo apt update
```

## Deleting Docker Desktop.

* I use the `apt` command to remove docker-desktop:
    

```plaintext
sudo apt remove docker-desktop
```

* I recursively remove the hidden Docker Desktop directory (while leaving the Docker directory intact):
    

```plaintext
rm -r $HOME/.docker/desktop
```

* I remove the Docker CLI:
    

```plaintext
sudo rm /usr/local/bin/com.docker.cli
```

I use the apt command to purge Docker Desktop from the system:

```plaintext
sudo apt purge docker-desktop
```

## Downloading the Docker Desktop File.

* From a browser, I download the Docker Desktop `deb` file to the Download directory:
    

```plaintext
https://www.docker.com/products/docker-desktop/
```

* I install the following for non-Gnome desktops:
    

```plaintext
sudo apt install gnome-terminal
```

## Installing Docker Desktop.

* I change to the Download directory:
    

```plaintext
cd ~/Download
```

* I update the repo list:
    

```plaintext
sudo apt update
```

* I install Docker Desktop:
    

```plaintext
sudo apt install ./docker-desktop-<version>-<arch>.deb
```

> NOTE: I ignore the error message after `APT` completes the installation:  
> `N: Download is performed unsandboxed as root, as file '/home/user/Downloads/docker-desktop.deb' couldn't be accessed by user '_apt'. - pkgAcquire::Run (13: Permission denied)`

## Post-Docker Desktop Installation.

* I launch Docker Desktop from the Applications menu, or by running the following command:
    

```plaintext
sudo systemctl --user start docker-desktop
```

I check the local Docker installation with these two commands:

```plaintext
docker compose version
docker --version
```

* I enable Docker Desktop to start on boot (Settings &gt; General &gt; Start Docker Desktop when you log in):
    

```plaintext
systemctl --user enable docker-desktop
```

* I stop Docker Desktop from the Docker menu (select Quit Docker Desktop) by running the following:
    

```plaintext
systemctl --user stop docker-desktop
```

> NOTE: When a new version of Docker Desktop is released, all of these steps will need to re-run... Again.

## Preparing the Remote Docker Container for Docker Desktop.

Thanks to the private keys I installed on my `workstation` and pushed across the LAN, I now have a password-less connection to the Docker container running on my `homelab`. This opens the way for exploring multiple deployment options, remote access solutions, and distant distribution practices.

### Preparing the Remote Docker Service.

* From a `workstation` terminal, I login to the remote Docker container:
    

```plaintext
ssh -p '22' 'brian@192.168.188.[?]'
```

* I add a new group (which probably already exists):
    

```plaintext
sudo groupadd docker
```

* I add my (current, logged-in) account to the group (which is "brian", in this case):
    

```plaintext
sudo usermod -aG docker ${USER}
```

* I log out of the container:
    

```plaintext
exit
```

* I log back into the remote Docker container (which now recognises my account as a membership of the docker group):
    

```plaintext
ssh -p '22' 'brian@192.168.188.[?]'
```

### Running the "hello-world" App.

* I run the "hello-world" app without the `sudo` command, ensuring everything works:
    

```plaintext
docker run hello-world
```

* One way of fixing a "permission denied" error is to delete the hidden `.docker` directory (which will rebuild itself with the correct permissions):
    

```plaintext
sudo rm -R /home/${USER}/.docker
```

### Running the "getting-started" App.

* I then run the "getting-started" app:
    

```plaintext
docker run -d -p 80:80 docker/getting-started
```

I open a local `workstation` browser, and visit the "getting-started" app that is now running within the Docker container on port 80:

```plaintext
http://192.168.188.[?]:80
```

If the tutorial successfully loads in a browser, then it's time to install Portainer within the remote container.

### Installing Portainer in the Remote Container.

Portainer is a web-based user interface for managing Docker environments. It can be used to manage Docker containers, images, networks and volumes.

* I create a Docker volume to store the Portainer configuration data:
    

```plaintext
docker volume create portainer_data
```

* I pull the Portainer image from Docker Hub:
    

```plaintext
docker pull portainer/portainer-ce
```

* I run the Portainer container with the following command:
    

```plaintext
docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```

### Accessing Portainer Locally.

* I open a browser on my local `workstation` system, point it to the IP address for the remote container, and access Portainer on port 9000:
    

```plaintext
http://192.168.188.[?]:9000
```

After setting up the remote Portainer account, I now need to enable the API. This API is what Docker Desktop will use to connect to the remote container.

### Enabling the Docker API.

* I use Nano to open the `docker.service` file:
    

```plaintext
sudo nano /lib/systemd/system/docker.service
```

* I comment out (#) the existing `ExecStart` line, and replace it with the following line. Then I save the changes, and close Nano:
    

```plaintext
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://192.168.188.[?]:2375
```

* I reload the Docker daemon:
    

```plaintext
systemctl daemon-reload
```

* I stop Docker:
    

```plaintext
sudo systemctl stop docker.service
```

* I start Docker:
    

```plaintext
sudo systemctl start docker.service
```

* And I check the status:
    

```plaintext
service docker status
```

Once Docker successfully boots, I will connect the local Docker Desktop app to the remote Docker service running in the remote container.

### Installing the Portainer Extension in Docker Desktop.

* On the `workstation` system, I run Docker Desktop.
    
* In the left panel, I click the `⊕ Add Extensions` button.
    
* In the Browse tab I `Search` for, and install, the Portainer extension.
    
* I open the Potainer extension.
    

> NOTE: A local instance of Docker, the one associated with Docker Workbench, will be listed here.

* I click the `Environments` logo in the middle console (immediately under the `Home` logo near the top. I can expand the middle panel by clicking the right-pointing chevron (&gt;&gt;) at the top, the one that looks like a fast-forward icon.)
    
* I click the blue `+ Add environment` button.
    
* I select the `Docker Standalone` option and click the blue `Start Wizard` button at the bottom of the window.
    
* I select the `API` option, `Name` the environment "docker", specify the `Docker API URL` as "192.168.188.\[?\]:2375", and click the blue `Connect` button at the bottom of the page.
    
* The remote `docker` environment loads into the main panel, and can be closed with the `x` symbol of the new panel that has loaded into the middle console.
    
* To re-connect to my remote container, I click the Portainer extension on the left, and click the blue `Live connect` button for the `docker` environment.
    

## In Conclusion.

This post described how I install, and set, up Docker Desktop on an Ubuntu system. It also covered how I prepared a remote Docker container for use by Docker Desktop. I also explained the advantages of using Docker within an LXC environment. And I explained how I use the Portainer extension, within Docker Workbench, to connect to a remote Docker container. Other ways of connecting Docker Workbench to a Docker service exist. For me? This is The Way.

...Until Another Way proves better.

The advantages of building, and running, a Docker service within a container are numerous: Isolation and separation, portability and scalability, versioning and security, but most importantly, the services that run in containers are still accessible from the outside world.

I'm gradually assembling the components that I'll need to build 12 Startups. Each element in my constellation resembles an individual puzzle piece. Docker, Docker Desktop, Portainer, and the technologies I've covered (and will cover) in other posts should eventually coalesce around the gravitational force of my code. As the jigsaw pieces come together, we'll see a tapestry of coherence slowly unfold before our eyes. I'm looking forward to sharing more of my process with you, and I'm really excited about things coming together.

Until next time: Be safe, be kind, be awesome.