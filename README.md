# Docker
## _My project about docker_


Here's a step-by-step guide to install Docker on an Ubuntu 20.04 server:

Update your existing list of packages:

```sh
sudo apt update
```

Install a few prerequisite packages which let apt use packages over HTTPS:
```sh
   sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
Add the GPG key for the official Docker repository to your system:
```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
Add the Docker repository to APT sources:
```sh
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```
Update the package database with the Docker packages from the newly added repo:
```sh
sudo apt update
```
Make sure you are about to install from the Docker repo instead of the default Ubuntu repo:
```sh
apt-cache policy docker-ce
```
You should see output similar to the following, although the version numbers may be different:
```sh
docker-ce:
Installed: (none)
Candidate: 5:19.03.9~3-0~ubuntu-focal
Version table:
5:19.03.9~3-0~ubuntu-focal 500
         500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
```
Finally, install Docker:
```sh
sudo apt install docker-ce
```
Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it's running:
```sh
 sudo systemctl status docker
```
If you want to avoid typing sudo whenever you run the docker command, add your username to the docker group:
```sh
sudo usermod -aG docker ${USER}
```
To apply the new group membership, log out of the server and back in, or type the following:
```sh
su - ${USER}
```
Confirm that your user is now added to the docker group by typing:
```sh
 id -nG
```
Install Docker Compose:

1. Download the current stable release of Docker Compose:
```sh
sudo curl -L "https://github.com/docker/compose/releases/download/v2.2.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
2. Apply executable permissions to the binary:
```sh
sudo chmod +x /usr/local/bin/docker-compose
```
3. Optionally, install command completion for the bash shell:
```sh
sudo curl -L https://raw.githubusercontent.com/docker/compose/2.2.3/contrib/completion/bash/docker-compose -o /etc/bash_completion.d/docker-compose
```
4. Test the installation:
```sh
docker-compose --version
```
Save the configuration above to a file called `docker-compose.yml`.

Replace `zabbix_db_password` with a strong password of your choice for the Zabbix database.

Make sure Docker and Docker Compose are installed on your host.

Run `docker-compose up -d` to start up all containers in the background.

Notes:

The ports section in zabbix-web-nginx-pgsql exposes Zabbix frontend on port 80 of your Docker host. You can change this to a different port if port 80 is already in use.

The volumes in zabbix-agent bind-mount the host's root filesystem and the Docker socket so that Zabbix can monitor the host filesystem and Docker itself.

The privileged mode for zabbix-agent allows Zabbix to get detailed information from within the container about the host. However, use this with caution as it grants the container extended permissions.

After your services are up and running, you can access the Zabbix web interface by navigating to http://your-docker-host-ip/zabbix in a web browser. Use the default Zabbix credentials to log in (Admin / zabbix), and remember to change the admin password after your first login for security.